Architecture Design Document
TaskFlow — Lightweight Team Task Management Web App
Revision: 1.0
Author: Senior Solutions Architect
Date: 2026-03-13

1. Executive Summary
- Purpose: Provide a scalable, secure, maintainable architecture for TaskFlow that meets functional and non-functional requirements in the Product Specification (MVP in a 3‑month timebox).
- Scope: Covers system context, containers and components, API contracts, data model, infrastructure, security, scalability, observability, and Architecture Decision Records (ADRs).
- Key goals: Support core FRs (registration, auth, task CRUD, assignment, dashboard, notifications), <2s API responses under normal load, ~500 concurrent users, deployable via Docker on AWS or Azure, >=99.5% uptime in monitoring window.

2. Goals, Constraints, and Non-Goals
- Primary goals
  - Deliver MVP features FR-001..FR-010 per spec.
  - Rapid implementation using open-source stack: React, FastAPI, PostgreSQL, Docker.
  - Secure by default (HTTPS, JWT, password hashing).
  - Scalable to support ~500 concurrent users; straightforward horizontal scaling for future growth.
- Constraints
  - 3-month delivery window.
  - Prefer open-source tooling and managed cloud services (AWS/Azure).
  - Responsive UI for desktop/tablet only (no native mobile).
- Non-goals (MVP)
  - Advanced analytics and integrations.
  - Complex role-based access beyond creator/assignee/manager basics.
  - Mobile apps.

3. Requirements Mapping (high level)
- FR-001 Registration -> Frontend, Auth API, User service, Postgres
- FR-002 Login -> Frontend, Auth API, JWT issuer, Rate limiter
- FR-003 Create Task -> Frontend, Task API, Postgres
- FR-004 Assign Task -> Frontend, Task API, Assignment record, Notification Service
- FR-005 Edit Task -> Frontend, Task API, Authorization checks
- FR-006 Mark Completed -> Task API, Notification optional
- FR-007 Delete Task -> Task API (soft-delete), audit logs
- FR-008 Dashboard -> Task API list endpoints, pagination, caching (optional)
- FR-009 Filter Tasks -> Task API query parameters
- FR-010 Notifications -> Notification Service (queue), Email provider and in-app notifications

4. System Context (C4 - Level 1)
- Primary actors: Registered User (and Manager as privileged user)
- External systems: Email Provider (SMTP or SES), Identity store (internal), Monitoring (Prometheus/Grafana, Sentry), Cloud provider (AWS/Azure)
- System: TaskFlow (Frontend + Backend + Notification subsystem + Database + Queue)

PlantUML (C4 Context)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml
Person(user, "Registered User", "Uses TaskFlow to manage tasks")
Person(manager, "Manager", "Monitors team progress")
System(system, "TaskFlow", "Web application (React + FastAPI)")
System_Ext(email,"Email Provider","SMTP/SES")
System_Ext(mon,"Monitoring","Prometheus/Grafana/Sentry")
System_Ext(cloud,"Cloud Provider","AWS or Azure")
user -> system : Uses web UI
manager -> system : Uses web UI (additional permissions)
system -> email : Send notifications
system -> mon : Emits metrics/logs/alerts
system -> cloud : Deployment resources
@enduml
```

5. Container Architecture (C4 - Level 2)
- Containers:
  - Web Client (React SPA) — served via CDN or static hosting (S3/Cloud Storage + CloudFront/Cdn)
  - Backend API (FastAPI) — REST + GraphQL optional; issues JWTs and provides business APIs
  - Notification Worker(s) — background processors (Celery / RQ) to deliver email and in-app notifications
  - Message Queue — Redis (for RQ) or AWS SQS (managed) for decoupling notification delivery
  - PostgreSQL — primary data store (managed RDS / Azure Database)
  - Redis Cache — optional for caching and rate-limiting
  - CI/CD pipeline, Container Registry, Infrastructure as Code (Terraform)
  - Monitoring/Logging agents and services (Prometheus, Grafana, Sentry, ELK/Cloud Logging)

PlantUML (C4 Container)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
Person(user,"Registered User")
System_Boundary(taskflow,"TaskFlow") {
  Container(web,"Web Client (React)","Browser SPA","Static assets served via CDN")
  Container(api,"Backend API (FastAPI)","REST API","Business logic, JWT auth")
  Container(worker,"Notification Worker","Python worker","Processes queue, sends email/in-app")
  ContainerDb(db,"PostgreSQL","Relational DB","Persistent data: users, tasks, assignments")
  Container(queue,"Message Queue","Redis/RQ or SQS","Queues notification jobs")
  Container(cache,"Redis Cache","In-memory","Optional caching, rate-limiting")
}
System_Ext(email,"Email Provider (SMTP/SES)")
user -> web : Uses UI
web -> api : HTTPS (JWT)
api -> db : SQL
api -> queue : enqueue notification job
worker -> queue : dequeue
worker -> email : send
api <-> cache : optional
@enduml
```

6. Component Architecture (C4 - Level 3) — Backend API internals
- Backend API components:
  - API Gateway / HTTP layer (uvicorn + FastAPI)
  - Auth Module: registration, login, JWT generation & validation, rate-limiting
  - Users Service: user CRUD, profile, team membership
  - Tasks Service: tasks CRUD, status transitions, soft-delete handling, audit fields, optimistic locking (versioning)
  - Assignments Service: create assignment records, enforce team membership
  - Notifications Adapter: enqueues notification events with metadata
  - Persistence Layer: repositories/ORM (SQLAlchemy/Databases) and migrations (Alembic)
  - Background Job interface (RQ/Celery) connector
  - Observability & Health endpoints

PlantUML (C4 Component)
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml
Container(api,"Backend API (FastAPI)")
Component(auth,"Auth Module","Handles registration, login, JWT")
Component(users,"Users Service","User profile & teams")
Component(tasks,"Tasks Service","Task CRUD, validations, soft-delete")
Component(assign,"Assignments Service","Creates assignment records")
Component(notif,"Notification Adapter","Enqueues notifications")
Component(repo,"Persistence (ORM/SQL)","SQLAlchemy + Alembic")
Component(health,"Health & Metrics","Liveness/readiness / Prometheus metrics")
api -> auth : REST calls
api -> users : REST calls
api -> tasks : REST calls
api -> assign : REST calls
tasks -> repo : read/write
users -> repo : read/write
assign -> repo : read/write
auth -> repo : read/write
api -> notif : enqueue
health <- api : expose metrics
@enduml
```

Component responsibilities and boundaries
- Web Client (React)
  - Responsibilities: Present UI, input validation, store short-lived JWT securely, call backend APIs, show notifications, manage session.
  - Boundary: No direct DB or queue access. All data via API.
- Backend API (FastAPI)
  - Responsibilities: Authorize requests, validate, enforce business rules, persist data, enqueue notifications, return RFC-like consistent error responses.
  - Boundary: Trusted environment, interacts with DB, queue, monitoring services.
- Notification Worker
  - Responsibilities: Reliable delivery of emails/in-app notifications, retries with exponential backoff, mark delivery status, log failures and metrics.
  - Observability: record delivery metrics and errors.
- Data stores
  - PostgreSQL: authoritative store, ACID transactions for tasks and assignments.
  - Redis/SQS: transient queue; Redis optionally used for caching and rate-limiting.

7. API Design (Contracts)
- API conventions
  - Base path: /api/v1
  - All endpoints HTTPS only.
  - Authentication: Authorization: Bearer <access_token> (JWT). Access token short-lived (e.g., 15 minutes). Implement refresh tokens as HttpOnly secure cookie (recommended).
  - Response format: JSON. Errors follow Problem Details (RFC7807) structure for consistent error codes and messages.
  - Pagination: cursor (preferred) or limit/offset; include next_cursor, limit, total_estimate (optional).
  - Idempotency: For critical operations (assignment), permit client-provided Idempotency-Key header.
  - Rate limiting: 100 req/min per IP/user for auth endpoints; tuned per endpoint.

- Auth / User Endpoints
  - POST /api/v1/auth/register
    - Request:
      { "name": "Alice", "email": "alice@example.com", "password": "P@ssw0rd!" }
    - Response 201:
      { "user_id": "uuid", "email": "...", "created_at": "...", "token": "<jwt>" }
    - Errors: 400 (validation), 409 (email exists), 500
  - POST /api/v1/auth/login
    - Request:
      { "email": "...", "password": "..." }
    - Response 200:
      { "user": { "user_id":"...", "name":"...", "email":"..." }, "token":"<jwt>", "expires_in":900 }
    - Rate-limited: 429 for too many attempts
  - GET /api/v1/users/me
    - Headers: Authorization: Bearer <jwt>
    - Response 200: current profile and team membership.

- Task Endpoints
  - POST /api/v1/tasks
    - Auth required
    - Request:
      { "title":"...", "description":"", "priority":"low|medium|high", "assignee_id":"uuid (optional)" }
    - Response 201:
      { "task_id":"uuid", "title":"...", "status":"open", "created_by":"user_id", "created_at":"..." }
    - Behavior: If assignee_id provided, validate user exists and team membership, create assignment record, enqueue notification.
  - GET /api/v1/tasks
    - Query params: status, assignee_id, priority, cursor, limit, sort_by
    - Response 200: { "items":[{task}], "next_cursor":"...", "limit":50 }
    - Performance: server-side filtering & pagination
  - GET /api/v1/tasks/{task_id}
    - Response 200: full task
  - PATCH /api/v1/tasks/{task_id}
    - Request: Partial fields (title, description, priority, status) + optional version token for optimistic concurrency
    - Response 200: updated task
    - Possible 403 (unauthorized), 409 (conflict)
  - DELETE /api/v1/tasks/{task_id}
    - Soft-delete: sets deleted_at; Response 204
    - Authorization: creator or admin

- Assignment Endpoints
  - POST /api/v1/tasks/{task_id}/assign
    - Request:
      { "assignee_id":"uuid" }
    - Response 201:
      { "assignment_id":"uuid", "task_id":"...", "assignee_id":"...", "assigned_at":"..." }
    - Behavior: Enqueue notification; handle last-writer-wins or optional versioning.
  - GET /api/v1/users/{user_id}/assignments
    - Returns assignments for user (with pagination)

- Notification Endpoints (internal / admin)
  - GET /api/v1/notifications
    - Query unread, limit, cursor
  - PATCH /api/v1/notifications/{id}/mark_read
    - Response 200

- Error model (example)
  - 400 Bad Request
    {
      "type": "/errors/validation",
      "title": "Validation Error",
      "status": 400,
      "detail": "Title is required",
      "errors": { "title": ["required"] }
    }
  - 401 Unauthorized, 403 Forbidden, 409 Conflict, 429 Too Many Requests, 500 Internal

8. Data Architecture
- Logical schema (concise)
  - users
    - id (UUID PK), name, email (unique), password_hash, role (user/manager/admin), team_id, created_at, updated_at, disabled_at
    - Index: email (unique)
  - teams (optional)
    - id (UUID), name, created_at
  - tasks
    - id (UUID), title, description, status ENUM (open,in_progress,completed,archived), priority ENUM, created_by FK users(id), assignee_id FK users(id) (nullable), created_at, updated_at, completed_at (nullable), deleted_at (nullable), version (integer)
    - Indexes: status, created_by, assignee_id, (created_at)
  - assignments
    - id (UUID), task_id FK tasks(id), assignee_id FK users(id), assigned_by FK users(id), assigned_at timestamp
    - Index: assignee_id
  - notifications
    - id (UUID), user_id FK, type (assignment...), payload JSONB, delivered boolean, delivery_attempts integer, last_attempt_at, created_at
  - audit_logs
    - id, actor_id, action, resource_type, resource_id, metadata JSONB, created_at
- Persistence patterns
  - Use SQL transactions for create+assign operations to ensure assignment consistent.
  - Soft-delete by setting deleted_at; exclude by default in queries.
  - Optimistic concurrency: tasks.version integer incremented on writes; clients can send If-Match version header to detect concurrent edits (return 409 when mismatch).
- Backups & retention
  - Managed DB daily backups (point-in-time recovery for 7–30 days depending on SLA)
  - Soft-deletes retained for configurable retention window (e.g., 30 days) before permanent purge.

9. Infrastructure & Deployment
- Environments: dev, staging, prod
- Containerization: Docker images for Backend and Worker. Frontend built as static assets.
- CI/CD: GitHub Actions / Azure DevOps / GitLab CI pipeline
  - Build → run unit tests → build images → push to registry → deploy to environment via IaC.
- Deployment targets (choice; see ADRs)
  - Recommended: AWS Fargate (ECS) or Azure Container Instances + managed RDS/Redis for MVP for fast managed operations. Alternatively AKS/EKS if Kubernetes expertise available.
- Infrastructure as Code: Terraform for cloud resources and managed services.
- Secrets: Managed secrets (AWS Secrets Manager / Azure Key Vault)
- Service discovery & Load balancing: Managed load balancer (ALB / Azure Application Gateway); TLS termination at LB; enforce HTTPS.
- Horizontal scaling
  - API and Worker scaled by container tasks (CPU/memory and autoscaling rules)
  - Postgres scaled vertically + read replicas if needed
  - Queue: SQS or Redis (managed Elasticache)
- Local dev: Docker Compose setup (Postgres, Redis, API, Worker, Frontend via dev server)

10. Security Architecture
- Authentication & Authorization
  - JWT access tokens signed with strong key (RS256 recommended) and short expiry (~15m). Use refresh tokens as HttpOnly Secure SameSite cookies issued at login (longer-lived, revocable).
  - Passwords hashed with Argon2id or bcrypt (work factor tuned). Store only password_hash.
  - Account lockout and rate-limiting for auth endpoints.
  - Authorization: RBAC minimal (creator, assignee, manager). Enforce at API layer per endpoint.
- Transport & Network
  - TLS 1.2+ enforced for all services and CDN.
  - Internal services communicate over private networks (VPC/subnet). No public DB access.
- Secrets & Keys
  - Store in cloud KMS/Secrets Manager.
  - Rotate keys on schedule; rotate signing keys carefully and support key versions in JWT validation.
- Data protection
  - Encrypt data at rest (managed DB encryption).
  - Regular backups and point-in-time recovery.
  - Soft-delete and audit logs to prevent accidental permanent deletion.
- Injection & OWASP mitigation
  - Use parameterized queries/ORM.
  - Input validation, rate limiting, CSP headers, X-Frame-Options, secure cookies.
- Logging & PII
  - Do not log passwords or full JWTs.
  - Mask/email hashing where necessary in logs.
- Security testing
  - Dependency scanning, SAST tools, basic dynamic scans for auth flows, third-party security review before launch.

11. Observability, Monitoring & Alerting
- Metrics
  - Expose Prometheus metrics from API & Worker (request rates, latencies, error rates, DB pool usage, queue length, job failures).
  - Dashboards: Grafana for system health and business metrics (tasks created, assignments queued).
- Logging
  - Structured JSON logs shipped to central logging (ELK or Cloud Logging). Include request IDs and trace ids.
- Tracing
  - Distributed tracing (OpenTelemetry) to trace request across API and worker for assignment flow.
- Alerts
  - High error rate, high latency (>2s median for list endpoints), queue backlog growth, failed job rate, DB connection exhaustion, low disk.
- Health checks
  - Readiness and Liveness endpoints. Load balancer configured to use health probes.
- SLO & Uptime
  - Target >=99.5% uptime for monitoring window. SLOs implemented with alert thresholds.

12. Scalability and Performance Considerations
- Expected load: 500 concurrent users. Design for horizontal scaling.
- Performance techniques
  - Database: proper indexing (status, created_by, assignee_id), query pagination and limits, optimistic paging.
  - Caching: Redis for frequently-read endpoints (dashboard list) with short TTLs and cache invalidation on updates.
  - Connection pooling: configure DB pools to match instance capacity.
  - Backgrounding: notifications use queueing not synchronous sends.
  - Load testing: simulate 500 concurrent users for listing/filtering endpoints; tune accordingly.
- Concurrency controls
  - Use optimistic locking (version) for concurrent edits; last-writer-wins only if acceptable, but recommend version-based conflict detection (409) for edits and assignment operations.
- Bottlenecks to watch
  - DB write spikes on assignment bursts — consider write scaling or batching.
  - Email provider rate limits — ensure worker throttles outbound sends.

13. Reliability & Disaster Recovery
- Backups: daily snapshots and point-in-time recovery for DB.
- Multi-AZ deployment for DB and compute where possible.
- Idempotency & retries: job processing with exponential backoff and dead-letter queue for failures.
- Restore procedure: documented restore from backup to separate cluster, alerting and failover test schedule.

14. Testing Strategy
- Unit tests for business logic, auth, validation.
- Integration tests against test DB/containers for API and worker.
- End-to-end tests (Cypress) covering registration, login, create/assign/complete flows.
- Load tests using k6 or Locust to validate 500 concurrent users and response targets.
- Security tests: dependency vulnerability scanning and basic OWASP checks.

15. Operational Runbook (high level)
- Deploy rollback: CI pipeline supports rolling back to last known-good image tag and DB migrations are backward compatible.
- Incident response: paging for severe alerts, runbooks for queue backlog, DB connection exhaustion, unhealthy workers.
- Backup & restore playbooks maintained in repository.

16. Risks & Mitigations (top items from spec, with system edit)
- Notification delivery failure
  - Mitigation: queue with retries, fallback to in-app, monitoring on delivery metrics, dead-letter queue and alerting.
- Security misconfig
  - Mitigation: enforce TLS, use managed secrets, code review, security scan, limited production secrets exposure.
- Performance under concurrent load
  - Mitigation: load tests, caching, indexing, autoscaling rules, performance budget for endpoints.
- Data loss
  - Mitigation: soft-delete, backups, audit logs.
- Timeline
  - Mitigation: strict MVP scope, incremental delivery, prioritize FRs in Implementation Notes.

17. Implementation Plan & Prioritization (MVP)
- Sprint 1: Project scaffolding, CI/CD, infra IaC (dev), user registration/login (FR-001, FR-002), DB schema
- Sprint 2: Task create/list/view (FR-003, FR-008), pagination/filtering base (FR-009 minimal)
- Sprint 3: Assignment & Notifications (FR-004, FR-010) — queue and worker
- Sprint 4: Complete/edit/delete flows (FR-005, FR-006, FR-007), optimistic concurrency, soft-delete
- Sprint 5: Observability, load testing, security hardening, staging verification, production deployment

18. Technology Stack (Rationale)
- Frontend: React (fast developer productivity, ecosystem), build to static assets served via CDN
- Backend: FastAPI (Python) — high productivity, async support, type hints, good for quick MVP; aligns with OpenAPI generation
- Database: PostgreSQL (managed RDS / Azure DB) — ACID, JSONB for notification payloads, reliable backups
- Background jobs & queue: RQ (Redis) or Celery (RabbitMQ/Redis) or managed SQS + Lambda for simpler ops. Recommended: AWS SQS + AWS Lambda or small worker pool with RQ/Redis for MVP. Choice recorded in ADR.
- Caching & rate-limiting: Redis (managed Elasticache or Azure Cache)
- Containerization & Orchestration: Docker, deploy to AWS ECS/Fargate or Azure Container Instances for speed; use Kubernetes (EKS/AKS) only if team has maturity.
- Email: AWS SES or external SMTP provider
- Observability: Prometheus + Grafana, OpenTelemetry tracing, Sentry for error tracking
- IaC: Terraform
Rationale: Stack prioritizes developer speed, open-source components, and managed services for operational simplicity given 3-month timeline.

19. Architecture Decision Records (ADRs)
ADR-001: Backend Framework: FastAPI chosen
- Status: Accepted
- Context: Need quick MVP, async I/O for IO-bound operations (DB, email), OpenAPI support.
- Decision: Use FastAPI (Python) + uvicorn.
- Alternatives: Node.js/Express, Django, Go.
- Rationale: Fast development, type-safety, async support, good ecosystem for background workers, aligns with team skillset assumption.
- Consequences: Leverage Python tooling; ensure concurrency tuning (uvicorn workers + gunicorn if needed).

ADR-002: Authentication: JWT access tokens + refresh tokens
- Status: Accepted
- Context: Requirement: short-lived JWT, secure login.
- Decision: Issue short-lived signed JWT (RS256) for API access; use HttpOnly secure refresh tokens in cookie for session renewal.
- Alternatives: Stateful sessions, OAuth provider.
- Rationale: Stateless scaling, aligns with RESTful API and simple frontend.
- Consequences: Need secure key management, token revocation strategy (maintain refresh token revocation store).

ADR-003: Notification Queue: Managed SQS vs Redis/RQ
- Status: Accepted (SQS recommended on AWS; Redis/RQ alternative for faster local dev)
- Context: Need reliable queue for notifications with retries and dead-letter handling.
- Decision: Use AWS SQS (managed) in production; Redis + RQ for local/dev and small-scale deployments.
- Alternatives: Celery + RabbitMQ, direct SMTP synchronous sends.
- Rationale: SQS removes operational burden, supports scaling and DLQ. RQ enables fast local iterations and simple worker model.
- Consequences: Production operations simplified; must abstract queue layer to swap provider.

ADR-004: Deployment Target: Managed containers (AWS Fargate / Azure Container Instances)
- Status: Accepted
- Context: Need deployable on AWS/Azure and low ops overhead for MVP.
- Decision: Use AWS Fargate (ECS) or Azure Container Instances / App Service for containers. If Kubernetes expertise exists, use EKS/AKS later.
- Alternatives: Full Kubernetes, serverless (Lambda)
- Rationale: Faster to deliver, managed autoscaling, less cluster ops.
- Consequences: Simpler operational model; if complex routing/sidecars needed later, may require migration.

ADR-005: Data Model: Soft-delete + optimistic concurrency
- Status: Accepted
- Context: Requirement: soft-delete, audit logs, concurrency conflict handling.
- Decision: Implement deleted_at soft-delete and integer version field for optimistic locking. Return 409 on version mismatch.
- Alternatives: Hard delete, DB MVCC strategies
- Rationale: Prevent accidental data loss, allow recovery; versioning handles concurrency conflicts elegantly.
- Consequences: Queries must filter deleted_at IS NULL by default; migrations for purge jobs.

ADR-006: Password hashing algorithm
- Status: Accepted
- Context: Security requirements
- Decision: Use Argon2id with reasonable parameters; fallback to bcrypt if Argon2 not available.
- Alternatives: bcrypt, scrypt.
- Rationale: Argon2id recommended by contemporary standards for password hashing.
- Consequences: Need to manage dependencies and tune memory/iterations for deployment environment.

20. Appendix
- Sample infrastructure components
  - AWS: VPC, ALB, ECS Fargate (API & Worker), RDS Postgres, ElastiCache Redis (dev), SQS, SES, CloudWatch, Secrets Manager.
  - Azure: Virtual Network, Application Gateway, Azure Container Instances/AKS, Azure Database for PostgreSQL, Azure Cache for Redis, Service Bus, SendGrid or SMTP, Azure Monitor, Key Vault.
- Local dev stack (Docker Compose)
  - services: api, worker, postgres, redis, web (dev server)
- CI/CD: include migrations run step (Alembic) with automated dry-run on staging and manual approval for prod migrations.
- Sample metrics to monitor
  - API request latency (p95, p99)
  - Task list endpoint median latency
  - Queue depth and job failure rate
  - DB connections used
  - Auth failures / brute-force patterns
  - Notification delivery success rate

21. Implementation Checklist (MVP)
- [ ] IaC skeleton and dev environment
- [ ] Auth registration & login (tests)
- [ ] User model and teams
- [ ] Task CRUD and listing with pagination (tests)
- [ ] Assignment endpoint and persistence (tests)
- [ ] Notification queue + worker + email/in-app delivery
- [ ] Soft-delete + audit logs
- [ ] Rate limiting on auth endpoints
- [ ] Observability (metrics/logging/tracing)
- [ ] Load testing and performance tuning
- [ ] Security scanning and basic pentest checklist
- [ ] Production deployment & monitoring with alerts

22. Glossary
- JWT: JSON Web Token
- DLQ: Dead Letter Queue
- SLO: Service Level Objective
- RBAC: Role-Based Access Control
- RQ/Celery: Python background job frameworks

End of Architecture Design Document

If you want, I can:
- Produce PlantUML PNG/SVG renderables for C4 diagrams,
- Generate OpenAPI (Swagger) skeleton for the API endpoints above,
- Produce Terraform snippets for the recommended AWS deployment,
- Or a sample Postgres schema SQL and migration (Alembic) for the data model.