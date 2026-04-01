# E2E Test Plan: TaskFlow | TaskFlow

## 1. Test Objectives
- Validate secure, reliable user authentication and session lifecycle (FR-001, FR-002, NFR-003).
- Validate critical user journeys end-to-end: registration → login → dashboard, create → assign → notify, edit concurrency, complete, delete/undelete (FR-001..FR-010; UC-001..UC-010).
- Mitigate risks around data integrity, concurrency, notification latency, and performance at scale (TR-Auth, TR-Notification, NFR-001, DR-Retention).

## 2. Scope

### In Scope
| Category | Items | Requirement IDs |
|----------|-------|-----------------|
| Functional | Registration, Authentication, Task Create/Edit/Assign/Complete/Delete, Dashboard, Filters, Notifications | FR-001, FR-002, FR-003, FR-004, FR-005, FR-006, FR-007, FR-008, FR-009, FR-010 |
| User Journeys | Registration→Login→Dashboard; Create→Assign→Notify; Create→Concurrent Edit→Conflict; Delete→Undelete; Dashboard filtering/pagination | UC-001, UC-002, UC-003, UC-004, UC-005, UC-006, UC-007, UC-008, UC-009, UC-010 |
| Non-Functional | Performance (P95 latency), Security (password hashing, tokens), Scalability, Availability, Observability, Accessibility | NFR-001, NFR-002, NFR-003, NFR-004, NFR-005, NFR-006, NFR-007 |
| Technical | JWT lifecycle, refresh rotation, email provider integration, notification enqueueing, DB migrations and backup/restore | TR-Auth, TR-Email, TR-Notification, TR-DB, TR-Cache |
| Data | Referential integrity, soft-delete retention and undelete, audit trails, PII handling | DR-DataIntegrity, DR-Retention, DR-Privacy, DR-Migrations |
| AI Models | None for MVP (no AIR-XXX present) | N/A |

### Out of Scope
- Advanced analytics, AI features, mobile native apps, third-party PM tool integrations — intentionally excluded for MVP to focus on core deterministic flows and meet delivery timeline.

## 3. Test Strategy

### Test Pyramid Allocation
| Level | Coverage Target | Focus |
|-------|-----------------|-------|
| E2E | 5-10% | Critical user journeys only (UI and API-driven full-stack validations) |
| Integration | 20-30% | API contracts, DB interactions, notification queueing, email provider contract tests |
| Unit | 60-70% | Business logic, validation, token handling, optimistic locking, utility functions |

### E2E Approach
- Horizontal: UI-driven flows for high-value user journeys (registration/login/dashboard; create → assign → notify).
- Vertical: Headless API → DB validations to assert data integrity (assignment records, audit trails, deletion flags).
- Use API-driven E2E (fast) where UI flakiness risks increase; reserve UI E2E for one representative path per journey.

### Environment Strategy
| Environment | Purpose | Data Strategy |
|-------------|---------|---------------|
| DEV | Developer iteration, unit and fast integration tests | Mocked services, in-memory DB or test Postgres seeded with minimal fixtures |
| QA | Full regression, integration & E2E | Seeded snapshot data with controlled volumes; email sandbox and notification service enabled |
| STAGING | Pre-prod performance/security validation | Prod-like data volumes; provider sandbox accounts; enable backups and monitoring |
| PROD (smoke) | Post-deploy smoke checks | Non-prod synthetic users + monitoring; no test data persisted to production datasets |

## 4. Test Cases

### 4.1 Functional Test Cases

#### TC-FR-001-01: User Registration - Happy Path
| Field | Value |
|-------|-------|
| Requirement | FR-001 |
| Use Case | UC-001 |
| Type | happy_path |

**Preconditions:**
- System available in QA environment.
- No existing user with test email.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given registration page accessible | When user submits valid name, unique email, and password meeting policy | Then API returns 201 Created and user record persisted |
| 2 | Given user persisted | When user attempts to login with provided credentials | Then login succeeds (FR-002 validated later) and user redirected to dashboard |
| 3 | Given registration completed | When inspect DB | Then password stored hashed (Argon2/bcrypt) and created_at present |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| email | test.user+1@example.com | test.user@example (bad format) | max length email per system |
| password | Str0ngPassword! (>=10, letters+numbers) | short (abc) | exactly 10 chars |

**Expected Results:**
- [ ] 201 Created returned
- [ ] Password_hash exists and not equal to plaintext
- [ ] No plaintext password in logs

**Postconditions:**
- Test user created and removable by teardown script

---

#### TC-FR-001-02: User Registration - Duplicate Email
| Field | Value |
|-------|-------|
| Requirement | FR-001 |
| Use Case | UC-001 |
| Type | error |

**Preconditions:**
- User with email already exists.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given existing user email in DB | When registration attempted with same email | Then API returns 409 Conflict with descriptive message |
| 2 | Given 409 response | When inspect DB | Then no duplicate user created |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| email | existing@example.com | existing@example.com | - |

**Expected Results:**
- [ ] 409 Conflict returned
- [ ] Error message indicates duplicate email
- [ ] No duplicate row added

**Postconditions:**
- DB unchanged

---

#### TC-FR-002-01: Authentication - Successful Login & Token Issuance
| Field | Value |
|-------|-------|
| Requirement | FR-002 |
| Use Case | UC-002 |
| Type | happy_path |

**Preconditions:**
- Test user exists with hashed password.
- Rate limiter at baseline.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given valid credentials | When POST /api/login | Then 200 OK returns access token (JWT) and refresh token (HttpOnly cookie or token) |
| 2 | Given access token | When calling protected endpoint | Then request authorized and resource returned |
| 3 | Given refresh token near expiry | When calling refresh endpoint | Then new access token issued and refresh rotation applied per policy |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| email | test.user+login@example.com | wrong@example.com | - |
| password | Str0ngPassword! | WrongPass | - |

**Expected Results:**
- [ ] Access token is JWT with 15m expiry claim
- [ ] Refresh token set Secure + HttpOnly when cookie mechanism used
- [ ] Protected API responds 200 with valid token

**Postconditions:**
- Session tokens issued and usable

---

#### TC-FR-002-02: Authentication - Invalid Credentials & Rate Limiting
| Field | Value |
|-------|-------|
| Requirement | FR-002 |
| Use Case | UC-002 |
| Type | error |

**Preconditions:**
- Rate limiter configured (e.g., 10 attempts / 10min threshold).

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given invalid credentials | When POST /api/login | Then 401 Unauthorized and failed_attempts incremented |
| 2 | Given repeated invalid attempts (simulate 11 attempts) | When threshold exceeded | Then account locked or 429 per IP and login returns 423 Locked or similar with unlock guidance |
| 3 | Given locked account | When valid credentials used | Then access denied until unlock; user receives lockout message |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| password | Str0ngPassword! | wrongpass | repeated attempts = threshold |

**Expected Results:**
- [ ] 401 for invalid creds
- [ ] Lockout behavior after threshold: 423/429 and descriptive message
- [ ] No token issued on invalid or locked attempts

**Postconditions:**
- Failed attempt counters reset per TTL or manual reset during teardown

---

#### TC-FR-003-01: Create Task - Happy Path
| Field | Value |
|-------|-------|
| Requirement | FR-003 |
| Use Case | UC-003 |
| Type | happy_path |

**Preconditions:**
- Authenticated user with valid access token.
- User belongs to a team.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given authenticated session | When POST /api/tasks with valid title (<=255), optional description and priority | Then 201 Created with task_id and created_at |
| 2 | Given task created | When GET /api/tasks/{id} | Then task shows status="New" and created_by equals requester |
| 3 | Given task created | When query dashboard list | Then task appears per permission scope |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| title | "Fix login bug" | "" (empty) | 255-char string |
| description | "Details..." | >4000 chars | 0 chars (optional) |
| priority | "High" | "Urgent" (unsupported) | default Medium |

**Expected Results:**
- [ ] 201 Created returned with task_id
- [ ] created_by recorded correctly
- [ ] Default status = New

**Postconditions:**
- Task persisted and available for assign/edit tests

---

#### TC-FR-003-02: Create Task - Validation Errors
| Field | Value |
|-------|-------|
| Requirement | FR-003 |
| Use Case | UC-003 |
| Type | error |

**Preconditions:**
- Authenticated user.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given empty title | When POST /api/tasks | Then 400 Bad Request with validation details |
| 2 | Given description >4000 | When POST /api/tasks | Then 400 Bad Request with field error |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| title | "T" | "" | 255 chars |
| description | "D" | 4001 chars | 4000 chars |

**Expected Results:**
- [ ] 400 returned with field-specific messages
- [ ] No task created

**Postconditions:**
- No DB records for invalid inputs

---

#### TC-FR-004-01: Assign Task - Happy Path & Notification Enqueue
| Field | Value |
|-------|-------|
| Requirement | FR-004, FR-010 |
| Use Case | UC-004, UC-009 |
| Type | happy_path |

**Preconditions:**
- Task exists and is not soft-deleted.
- Assignee exists and is member of same team.
- Assignee has email opt-in ON.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given creator authorized | When POST /api/tasks/{id}/assign {assignee_id} | Then 200 OK and Assignment record created with assigned_at |
| 2 | Given assignment created | When inspect Notification queue | Then in-app notification enqueued for assignee |
| 3 | Given email opt-in | When notification processed | Then email entry queued to email provider and content includes task title, assigner name, link, timestamp |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| assignee_id | user-team-member-uuid | user-other-team-uuid | - |
| notification preference | ON | OFF | - |

**Expected Results:**
- [ ] 200 OK
- [ ] Assignment persisted; assignment history query returns record
- [ ] Notification queued and eventual delivery logged (simulated by email sandbox)

**Postconditions:**
- Assignment history persists; notification log entry created

---

#### TC-FR-004-02: Assign Task - Assignee Not in Team (Error)
| Field | Value |
|-------|-------|
| Requirement | FR-004 |
| Use Case | UC-004 |
| Type | error |

**Preconditions:**
- Task and target user exist but are in different teams.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given assignee in different team | When POST /api/tasks/{id}/assign | Then 400 Bad Request with descriptive error; no assignment record |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| assignee_id | other-team-user | - | - |

**Expected Results:**
- [ ] 400 returned
- [ ] No Assignment record created

**Postconditions:**
- System state unchanged

---

#### TC-FR-005-01: Edit Task - Happy Path with Version Match
| Field | Value |
|-------|-------|
| Requirement | FR-005 |
| Use Case | UC-005 |
| Type | happy_path |

**Preconditions:**
- Authenticated user authorized to edit (creator or assignee).
- Client holds current version/ETag of task.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given current version = V | When PATCH /api/tasks/{id} with version=V and updated fields | Then 200 OK and version increments |
| 2 | Given change persisted | When GET /api/tasks/{id} | Then updated fields visible and audit record created |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| title | "Updated title" | >255 chars | 255 chars |
| version | 3 | 999 (stale) | current version |

**Expected Results:**
- [ ] 200 OK with new version
- [ ] Audit trail records editor and timestamp

**Postconditions:**
- Updated task saved; version incremented

---

#### TC-FR-005-02: Edit Task - Version Mismatch (Conflict)
| Field | Value |
|-------|-------|
| Requirement | FR-005 |
| Use Case | UC-005 |
| Type | error |

**Preconditions:**
- Two clients fetched same task version V.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given Client A and Client B have version V | When Client A updates with version V (succeeds) and Client B then attempts update with version V | Then Client B receives 409 Conflict with current version and diff hint |
| 2 | Given 409 returned | When client fetches latest version | Then client can merge or retry |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| version sequence | V | stale V | - |

**Expected Results:**
- [ ] 409 Conflict returned for stale update
- [ ] Server returns current version in response

**Postconditions:**
- No silent overwrite; data integrity maintained

---

#### TC-FR-006-01: Mark Completed - Idempotence & Metadata
| Field | Value |
|-------|-------|
| Requirement | FR-006 |
| Use Case | UC-006 |
| Type | happy_path/edge_case |

**Preconditions:**
- Task exists and user has permission.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given task status != Completed | When POST /api/tasks/{id}/complete | Then 200 OK, status set to Completed, completed_at set |
| 2 | Given task already Completed | When POST /api/tasks/{id}/complete again | Then 200 OK and completed_at unchanged |
| 3 | Given completion | When GET /api/tasks/{id} | Then completion metadata present and visible in dashboard filters |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| task status | "New" | "Archived" (deleted) | already "Completed" |

**Expected Results:**
- [ ] 200 OK on first and subsequent calls (idempotent)
- [ ] completed_at set only on first completion

**Postconditions:**
- Task marked Completed and reflected in DB

---

#### TC-FR-007-01: Soft Delete & Undelete - Happy Path
| Field | Value |
|-------|-------|
| Requirement | FR-007 |
| Use Case | UC-007 |
| Type | happy_path |

**Preconditions:**
- Task exists and user is creator or admin.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given authorized user | When DELETE /api/tasks/{id} | Then 200 OK and deleted_at + deleted_by set; task excluded from default dashboard |
| 2 | Given within retention window | When POST /api/tasks/{id}/undelete | Then 200 OK and deleted_at cleared; task visible again |
| 3 | Given retention window expired and hard-delete job ran | When GET /api/tasks/{id} | Then 404 Not Found |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| retention_days | 30 (default) | 0 (configured) | 30 |

**Expected Results:**
- [ ] deleted_at and deleted_by set on delete
- [ ] Task excluded from default queries
- [ ] Undelete restores task within retention window

**Postconditions:**
- Task restored or permanently removed per retention policy

---

#### TC-FR-008-01: Dashboard - Pagination, Sorting, Permission Scope
| Field | Value |
|-------|-------|
| Requirement | FR-008 |
| Use Case | UC-008 |
| Type | happy_path/edge_case |

**Preconditions:**
- Dataset seeded with >100 tasks across teams and priorities.
- Authenticated user with team-level scope.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given default request | When GET /api/tasks (no params) | Then returns page_size=25, sorted priority desc then created_at desc |
| 2 | Given query params page_size=10 & page_token | When paginating | Then consistent results with no duplicates across pages |
| 3 | Given user role without global access | When GET /api/tasks | Then only tasks visible for their team |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| page_size | 25 | 0 | 1, 100 |
| sort | priority_desc, created_at_desc | unsupported sort | - |

**Expected Results:**
- [ ] Default sort applied
- [ ] Pagination returns stable cursor-based pages
- [ ] Permission scoping enforced

**Postconditions:**
- Pagination state maintained and cursors usable

---

#### TC-FR-009-01: Filters - Combined Filters & Validation
| Field | Value |
|-------|-------|
| Requirement | FR-009 |
| Use Case | UC-008 |
| Type | happy_path/error |

**Preconditions:**
- Tasks with varied statuses, assignees, priorities and due_dates in DB.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given valid filters status=New&assignee=X | When GET /api/tasks?status=New&assignee=X | Then paginated results match filters and counts correct |
| 2 | Given malformed filter param | When GET /api/tasks?due_date=not-a-date | Then 400 Bad Request and descriptive message |
| 3 | Given combination of filters | When apply across pages | Then results remain consistent and P95 latency meets NFR under load (see performance tests) |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| status | New, Completed | UnknownStatus | - |
| due_date | 2026-04-30 | not-a-date | start=end |

**Expected Results:**
- [ ] Filtered results correct and paginated
- [ ] 400 on unsupported/malformed filters

**Postconditions:**
- No side effects

---

#### TC-FR-010-01: Notifications - In-app Latency & Email Retry
| Field | Value |
|-------|-------|
| Requirement | FR-010 |
| Use Case | UC-009 |
| Type | happy_path/edge_case |

**Preconditions:**
- Notification service and email provider sandbox reachable.
- Assignee opt-in ON.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given assignment event | When TS enqueues notification | Then in-app notification delivered to assignee within 5s for 95% of events under normal load |
| 2 | Given email attempt fails (simulate provider 5xx) | When Email provider returns error | Then system retries with exponential backoff up to 3 attempts and records failure in logs |
| 3 | Given user opted out | When assignment occurs | Then no email queued; in-app notification still created |

**Test Data:**
| Field | Valid Value | Invalid Value | Boundary Value |
|-------|-------------|---------------|----------------|
| retry_attempts | 3 | 0 | 3 |
| latency_threshold | <=5s (95th) | >5s | - |

**Expected Results:**
- [ ] In-app notification ≤5s for 95% of events (measured in QA under normal load)
- [ ] Email retries executed with backoff and logged
- [ ] Opt-out respected for email channel

**Postconditions:**
- Notification logs reflect delivery attempts and statuses

---

### 4.2 NFR Test Cases

#### TC-NFR-001-PERF: Dashboard Performance Validation
| Field | Value |
|-------|-------|
| Requirement | NFR-001 |
| Category | Performance |

**Preconditions:**
- System deployed in STAGING with prod-like dataset.
- Baseline load generator (k6) configured.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | System at baseline | When 500 concurrent virtual users execute representative dashboard queries | Then Response time P95 < 2s |
| 2 | System under load | When monitor error rates | Then Error rate < 1% (configurable threshold) |
| 3 | Peak load reached | When measure throughput | Then Throughput >= target req/sec required for SLA |

**Acceptance Criteria:**
- [ ] Response time P95 < 2s
- [ ] Error rate < 1%
- [ ] Throughput sustained per SLA
- [ ] No memory leaks observed in soak runs

---

#### TC-NFR-001-SCALE: Scalability Validation
| Field | Value |
|-------|-------|
| Requirement | NFR-001 |
| Category | Scalability |

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | System at baseline [100 users] | When ramp to 500 users | Then system scales horizontally without >10% performance degradation |
| 2 | Peak [500 users] sustained for 30 minutes | When monitor autoscaling | Then auto-scaling triggers and additional replicas handle load |
| 3 | Load reduced | When traffic drops | Then resources scale down and connections released properly |

**Acceptance Criteria:**
- [ ] Scales to 500 concurrent users
- [ ] Auto-scaling triggers and completes within configured window
- [ ] No degradation beyond acceptable limit

---

#### TC-NFR-002-AVAIL: Availability & Monitoring Validation
| Field | Value |
|-------|-------|
| Requirement | NFR-002 |
| Category | Availability |

**Preconditions:**
- Monitoring & alerting configured.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Synthetic monitor configured | When run daily health-check flows | Then uptime metrics collected and >99.5% over measurement window |
| 2 | Induce error spike (fault injection) | When error rate crosses threshold | Then alert fires and escalation works |
| 3 | For an incident | When trace/logging used | Then request traces available correlating request IDs |

**Acceptance Criteria:**
- [ ] Uptime ≥ 99.5% measured
- [ ] Alerts fired on error/latency thresholds
- [ ] Traces available for failed requests

---

#### TC-NFR-003-SEC: Security Validation (OWASP + Auth)
| Field | Value |
|-------|-------|
| Requirement | NFR-003 |
| Category | Security |

**Preconditions:**
- App endpoints accessible in STAGING.
- Test credentials available.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Authentication endpoints exposed | When attempt unauthorized access to protected endpoints | Then access denied (401/403) and no sensitive data leaked |
| 2 | Input fields available | When submit SQL/JS injection payloads | Then inputs sanitized / parameterized queries prevent injection |
| 3 | Session established | When inspect cookies | Then refresh cookies HttpOnly & Secure; access tokens short-lived (15m) |
| 4 | Run DAST (OWASP ZAP) | When scan complete | Then no Critical/High OWASP findings remain unmitigated |

**Acceptance Criteria:**
- [ ] No critical/high vulnerabilities open
- [ ] TLS enforced across endpoints
- [ ] Passwords never stored or logged in plaintext
- [ ] Authorization rules enforced for protected endpoints

---

### 4.3 Technical Requirement Test Cases

#### TC-TR-001: JWT Lifecycle & Refresh Token Rotation
| Field | Value |
|-------|-------|
| Requirement | TR-Auth |
| Category | Integration/API |

**Preconditions:**
- Auth service deployed.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given a valid access token | When token expires | Then API returns 401 for protected endpoints |
| 2 | Given valid refresh token | When POST /api/token/refresh | Then new access token issued and refresh token rotated per policy |
| 3 | Given rotated refresh token reused | When reuse occurs | Then previous refresh token invalidated and reuse rejected |

**Validation Points:**
- [ ] Token expiry enforced
- [ ] Refresh rotation implemented
- [ ] Cookie flags secure when used

---

#### TC-TR-002: Email Provider Integration & Retry Behavior
| Field | Value |
|-------|-------|
| Requirement | TR-Email, FR-010 |
| Category | Integration/API |

**Preconditions:**
- Email provider sandbox available (SES/SendGrid test account).
- Notification service configured to enqueue email events.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given a queued email | When provider returns 200 | Then delivery logged as success |
| 2 | Given provider returns 5xx | When system retries per exponential backoff | Then up to 3 attempts and final failure recorded |
| 3 | Given provider unreachable | When retry exhausted | Then failure recorded and admin alert/log created |

**Validation Points:**
- [ ] Retry/backoff executed
- [ ] Failure logged for later inspection

---

#### TC-TR-003: Notification Service - In-app Delivery (Websocket/Poll)
| Field | Value |
|-------|-------|
| Requirement | TR-Notification |
| Category | Integration/API |

**Preconditions:**
- Notification service and websocket endpoint available.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given assignee connected via websocket | When notification enqueued | Then notification pushed and visible on client UI |
| 2 | Given assignee not connected | When notification enqueued | Then notification stored and delivered on next poll/login |

**Validation Points:**
- [ ] Push and poll delivery paths both functional
- [ ] Delivery timestamp recorded

---

### 4.4 Data Requirement Test Cases

#### TC-DR-001: Data Integrity - Referential Integrity & Audit Trail
| Field | Value |
|-------|-------|
| Requirement | DR-DataIntegrity |
| Category | Integrity |

**Preconditions:**
- DB seeded with users, tasks, assignments.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given Task created | When Assignment created referencing task and user | Then FK constraints satisfied and assignment persisted |
| 2 | Given Task updated/deleted | When read assignment history | Then assignment history remains intact and points to historical data as required |
| 3 | Given edits | When audit trail queried | Then who/when/what entries present for each edit |

**Validation Points:**
- [ ] No orphaned assignment rows
- [ ] Audit trail contains required fields and timestamps

---

#### TC-DR-002: Retention & Backup/Restore
| Field | Value |
|-------|-------|
| Requirement | DR-Retention, NFR-004 |
| Category | Retention/Migration |

**Preconditions:**
- Backup job configured and can create snapshot.

**Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1 | Given DB snapshot created | When restore executed in isolated environment | Then restored dataset matches expected checksums and key records exist |
| 2 | Given soft-deleted items older than retention | When hard-delete scheduled job runs | Then records removed and associated indices/attachments cleaned |
| 3 | Given restore operation | When verify application-level integrity | Then referential integrity preserved and application can start against restored DB |

**Validation Points:**
- [ ] Restore succeeds and key records validated
- [ ] Retention job removes records after window

---

### 4.5 AI Requirement Test Cases [CONDITIONAL: If AIR-XXX in scope]
- Status: Not applicable — No AIR-XXX requirements present in requirements/design documents for MVP.
- Action: If AIR requirements are introduced later, apply TC-AIR templates and include relevance/hallucination/latency/safety checks per template.

### 4.6 E2E Journey Test Cases

#### E2E-001: Registration → Login → Dashboard
| Field | Value |
|-------|-------|
| UC Chain | UC-001 -> UC-002 -> UC-008 |
| Session | Guest → Authenticated |

**Preconditions:**
- QA environment accessible; email sandbox enabled.

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | UC-001 | Register new user | User record created; password hashed | Y |
| 2 | UC-002 | Login with credentials | Access & refresh tokens issued; dashboard accessible | Y |
| 3 | UC-008 | Dashboard load | Tasks list loads with default sort & pagination | Y |

**Detailed Test Steps:**

**Phase 1: UC-001 - Register**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given registration form open | When user submits valid details | Then 201 Created and DB contains user with hashed password |
| 1.2 | Given registration success | When check logs | Then no plaintext password logged |

**Phase 2: UC-002 - Login**
| Step | Given | When | Then |
|------|-------|------|------|
| 2.1 | Given user credentials | When POST /api/login | Then 200 OK and tokens returned |
| 2.2 | Given token returned | When call GET /api/tasks | Then 200 OK and dashboard accessible |

**Phase 3: UC-008 - Dashboard**
| Step | Given | When | Then |
|------|-------|------|------|
| 3.1 | Given tasks seeded | When GET /api/tasks | Then returns page_size=25 and default sorting |
| 3.2 | Given dashboard displayed | When user toggles sort | Then UI reflects changed ordering |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| User | Role | standard user |
| User | Credentials | seeded test_user@example.com / Str0ngPassword! |
| Tasks | Count | 50 seeded tasks varying priority/status |

**Expected Results:**
- [ ] User can register and login end-to-end
- [ ] Dashboard loads and respects sort/pagination

**Postconditions:**
- Test user and tasks removed or isolated in teardown

---

#### E2E-002: Create Task → Assign → Notification (In-app + Email)
| Field | Value |
|-------|-------|
| UC Chain | UC-003 -> UC-004 -> UC-009 |
| Session | Authenticated |

**Preconditions:**
- Creator and assignee users exist in same team; assignee email opt-in ON.

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | UC-003 | Create Task | Task persisted with created_by | Y |
| 2 | UC-004 | Assign Task | Assignment record created | Y |
| 3 | UC-009 | Notify Assignee | In-app notification visible; email queued/delivered | Y |

**Detailed Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given authenticated creator | When POST /api/tasks | Then 201 Created and task_id returned |
| 2.1 | Given task_id | When POST /api/tasks/{id}/assign {assignee_id} | Then 200 OK and assignment persisted |
| 3.1 | Given assignment | When notification service processes queue | Then in-app notification delivered within 5s and email queued; email content includes title, assigner, link, timestamp |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| Creator | email | creator@example.com |
| Assignee | email | assignee@example.com (opt-in) |
| Task | title | "E2E Task for Assignment" |

**Expected Results:**
- [ ] Assignee receives in-app notification within SLA
- [ ] Email queued and logged for delivery attempts

**Postconditions:**
- Assignment & notification logs persist

---

#### E2E-003: Create Task → Concurrent Edit → Conflict Handling
| Field | Value |
|-------|-------|
| UC Chain | UC-003 -> UC-005 |
| Session | Two authenticated users (Client A & Client B) |

**Preconditions:**
- Task exists and both clients fetch same version V.

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | UC-003 | Task fetched | Both clients hold version V | Y |
| 2 | UC-005 | Client A updates | Server accepts and version increments to V+1 | Y |
| 3 | UC-005 | Client B attempts update with V | Server returns 409 with current version | Y |

**Detailed Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given both clients fetched task V | When Client A PATCH with version V | Then 200 OK and version increments |
| 2.1 | Given Client A update succeeded | When Client B PATCH with stale version V | Then 409 Conflict and response contains current version and diff hint |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| Task | version | V (shared) |

**Expected Results:**
- [ ] Client B receives 409 Conflict
- [ ] Audit log records both attempts

**Postconditions:**
- No silent overwrite

---

#### E2E-004: Mark Completed → Dashboard Reflection → Idempotence
| Field | Value |
|-------|-------|
| UC Chain | UC-006 -> UC-008 |
| Session | Authenticated |

**Preconditions:**
- Task assigned and visible in dashboard.

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | UC-006 | Mark Completed | task status Completed and completed_at set | Y |
| 2 | UC-008 | Dashboard filter Completed | task included | Y |
| 3 | UC-006 | Re-call complete | idempotent (200 OK) | Y |

**Detailed Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given task status != Completed | When POST /api/tasks/{id}/complete | Then 200 OK and completed_at set |
| 2.1 | Given task Completed | When filter dashboard status=Completed | Then task appears |
| 3.1 | Given task Completed | When POST /api/tasks/{id}/complete again | Then 200 OK and completed_at unchanged |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| Task | status | New → Completed |

**Expected Results:**
- [ ] Completed metadata present and idempotence holds

**Postconditions:**
- Task remains Completed

---

#### E2E-005: Soft-delete → Exclusion from Dashboard → Undelete within Window
| Field | Value |
|-------|-------|
| UC Chain | UC-007 -> UC-008 |
| Session | Authenticated (creator/admin) |

**Preconditions:**
- Task exists and created by user.

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | UC-007 | Soft-delete task | deleted_at set; task excluded from default dashboard | Y |
| 2 | UC-007 | Undelete within 30 days | deleted_at cleared and task visible | Y |

**Detailed Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given task exists | When DELETE /api/tasks/{id} | Then 200 OK and deleted_at set |
| 2.1 | Given within retention | When POST /api/tasks/{id}/undelete | Then 200 OK and task restored |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| retention | days | 30 |

**Expected Results:**
- [ ] Task excluded post-delete and restored on undelete

**Postconditions:**
- Deleted and restored tasks cleaned up in teardown or left per retention policy

---

#### E2E-006: Dashboard Filtering & Pagination with Role Scoping
| Field | Value |
|-------|-------|
| UC Chain | UC-008 -> UC-009 |
| Session | Authenticated (team member) |

**Preconditions:**
- Seeded dataset across multiple teams and statuses.

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | UC-008 | Apply combined filters | Filtered result consistent across pages | Y |
| 2 | UC-008 | Change role or team membership | Assignable list updates immediately | Y |

**Detailed Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given filters status=New&priority=High | When GET /api/tasks | Then results match filters and pagination stable |
| 2.1 | Given membership change | When admin removes user from team | Then user no longer appears in assignee lists |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| Teams | A/B | memberships varied |
| Tasks | variety | status/priority matrix |

**Expected Results:**
- [ ] Filters combined correctly
- [ ] Permission scoping enforced after membership changes

**Postconditions:**
- Membership / filter states restored during teardown

---

#### E2E-007: Login Rate-Limit & Lockout Behavior
| Field | Value |
|-------|-------|
| UC Chain | UC-002 |
| Session | Unauthenticated |

**Preconditions:**
- Rate limiting set to threshold (10 attempts per 10 minutes).

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | UC-002 | Attempt repeated invalid logins | Account locked after threshold | Y |
| 2 | UC-002 | Valid credentials during lockout | Access denied until unlock | Y |

**Detailed Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given invalid credentials | When 11 invalid login attempts occur | Then account locked and 423 returned |
| 1.2 | Given lockout | When user follows unlock instruction | Then account unlocked and login succeeds |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| Attempts | count | threshold + 1 |

**Expected Results:**
- [ ] Lockout triggered and documented
- [ ] No token issued while locked

**Postconditions:**
- Lockout counter reset with TTL or admin intervention

---

#### E2E-008: Backup → Restore Validation (Ops Journey)
| Field | Value |
|-------|-------|
| UC Chain | NFR-004 (ops) |
| Session | Admin/ops |

**Preconditions:**
- Backup scheduled and snapshotable.

**Journey Flow:**
| Phase | Use Case | Action | Expected State | Checkpoint |
|-------|----------|--------|----------------|------------|
| 1 | Backup | Trigger backup snapshot | Snapshot created and verified | Y |
| 2 | Restore | Restore snapshot to isolated env | Restored dataset validated by checksum & key queries | Y |

**Detailed Test Steps:**
| Step | Given | When | Then |
|------|-------|------|------|
| 1.1 | Given DB state | When backup job runs | Then snapshot created successfully |
| 2.1 | Given snapshot | When restore to isolated STAGING-Restore | Then DB checksums and sample record counts match expected |

**Test Data:**
| Entity | Field | Value |
|--------|-------|-------|
| Sample data | rows | known counts |

**Expected Results:**
- [ ] Backup completes
- [ ] Restore yields expected dataset

**Postconditions:**
- Restored environment torn down after validation

---

## 5. Entry & Exit Criteria

### Entry Criteria
- [ ] All FR-XXX requirements approved and baselined
- [ ] Test environments (QA, STAGING) provisioned and accessible
- [ ] Test data seeded or available (seed/fixture scripts validated)
- [ ] Test cases reviewed and approved by QA lead and Product Owner
- [ ] Relevant NFR/TR/DR requirements reviewed and test harnesses available (k6, DAST, email sandbox)

### Exit Criteria
- [ ] 100% P0 test cases executed
- [ ] >=95% P0 test cases passed
- [ ] >=90% P1 test cases passed
- [ ] No open Critical/High severity defects
- [ ] NFR thresholds validated (performance P95, security checks)
- [ ] All defined E2E journeys pass end-to-end in QA and STAGING as applicable

## 6. Risk Assessment

| Risk-ID | Risk Description | Impact | Likelihood | Mitigation |
|---------|------------------|--------|------------|------------|
| R-001 | Broken authentication/token handling (e.g., refresh rotation failure) | High | Medium | P0 tests for token lifecycle; integration tests; security review; roll-back plan |
| R-002 | Notification latency > SLA causing poor UX | High | Medium | P0 E2E + perf tests for notification latency; queue monitoring and retry validation |
| R-003 | Concurrency leading to silent overwrites | High | Medium | P0 tests for optimistic locking; return 409 and UI guidance; versioning tested in integration |
| R-004 | Data loss during retention/hard-delete or failed backups | High | Low-Medium | Backup/restore validation (E2E-008); retention job test cases; alerting on job failures |
| R-005 | Email provider outages causing missed notifications | Medium | Medium | TR tests with provider sandbox and simulated failures; retries/backoff; record failures and admin alert |
| R-006 | Performance degradation at 500 concurrent users | High | Medium | NFR performance tests in STAGING; autoscaling and caching configuration validated |

### Risk-Based Test Prioritization
| Priority | Criteria | Test Focus |
|----------|----------|------------|
| P0 (Must Test) | Impact=High AND Likelihood>=Medium | Auth lifecycle( FR-002 ), Dashboard perf (FR-008 + NFR-001), Create→Assign→Notify path (FR-003, FR-004, FR-010), Edit concurrency (FR-005), Security/password hashing (FR-001, NFR-003) |
| P1 (Should Test) | Impact=Medium OR Likelihood=High | Soft-delete/undelete retention (FR-007, DR-Retention), Notification retry behavior (FR-010), Rate-limiting/lockout (FR-002), Pagination/filter correctness (FR-008/FR-009), Backup/restore (NFR-004) |
| P2 (Could Test) | Remaining scenarios | Admin hard-delete job, extended accessibility checks beyond core flows, maintainability metrics enforcement (NFR-007) |

## 7. Traceability Matrix

| Requirement | Type | Priority | Test Cases | E2E Journey | Status |
|-------------|------|----------|------------|-------------|--------|
| FR-001 | Functional | P0 | TC-FR-001-01, TC-FR-001-02 | E2E-001 | Planned |
| FR-002 | Functional | P0 | TC-FR-002-01, TC-FR-002-02 | E2E-001, E2E-007 | Planned |
| FR-003 | Functional | P0 | TC-FR-003-01, TC-FR-003-02 | E2E-002, E2E-003 | Planned |
| FR-004 | Functional | P0 | TC-FR-004-01, TC-FR-004-02 | E2E-002 | Planned |
| FR-005 | Functional | P0 | TC-FR-005-01, TC-FR-005-02 | E2E-003 | Planned |
| FR-006 | Functional | P1 | TC-FR-006-01 | E2E-004 | Planned |
| FR-007 | Functional | P1 | TC-FR-007-01 | E2E-005 | Planned |
| FR-008 | Functional | P0 | TC-FR-008-01 | E2E-001, E2E-006 | Planned |
| FR-009 | Functional | P1 | TC-FR-009-01 | E2E-006 | Planned |
| FR-010 | Functional | P0 | TC-FR-010-01 | E2E-002 | Planned |
| NFR-001 | Non-Functional | P0 | TC-NFR-001-PERF, TC-NFR-001-SCALE | - | Planned |
| NFR-002 | Non-Functional | P1 | TC-NFR-002-AVAIL | - | Planned |
| NFR-003 | Non-Functional | P0 | TC-NFR-003-SEC | - | Planned |
| NFR-004 | Non-Functional | P1 | TC-DR-002, E2E-008 | E2E-008 | Planned |
| TR-Auth | Technical | P0 | TC-TR-001 | E2E-001 | Planned |
| TR-Email | Technical | P1 | TC-TR-002 | E2E-002 | Planned |
| TR-Notification | Technical | P0 | TC-TR-003 | E2E-002 | Planned |
| DR-DataIntegrity | Data | P0 | TC-DR-001 | - | Planned |

Note: Priorities derived from Risk Assessment (Section 6).

## 8. Test Data Requirements

| Scenario Type | Data Description | Source | Isolation |
|---------------|------------------|--------|-----------|
| Happy Path | Valid users (admin, manager, team members), tasks with varied priorities/statuses, teams | Seeded fixtures | Test-specific DB/schema per environment |
| Edge Cases | Boundary titles/descriptions, expired tokens, stale versions | Generated | Shared read-only where safe |
| Error Cases | Invalid filter params, malformed payloads, non-team assignee | Static fixtures | Shared read-only |
| E2E Journeys | Complete user datasets for journeys (creator + assignee) | Journey-specific seed scripts | Journey-isolated DB schemas or namespaces |

### Sensitive Data Handling
- [ ] Production data masked/anonymized before use in non-prod.
- [ ] PII replaced with synthetic data in fixtures.
- [ ] Credentials stored in secret manager; test credentials read from env vars.
- [ ] Traces/screenshots scrubbed of tokens and PII before sharing.

## 9. Defect Management

| Severity | Definition | SLA | Action |
|----------|------------|-----|--------|
| Critical | System unusable, data loss, security breach | Immediate (block release) | Block release; hotfix & rollback plan |
| High | Major feature broken, no workaround, security vulnerability | Fix before release | Must fix; patch prioritized |
| Medium | Feature impacted, workaround exists | Next sprint | Should fix; scheduled in backlog |
| Low | Minor/cosmetic | Backlog | Could fix; documented for future sprints |

Defects tracked in JIRA (or preferred tracker) with labels: P0/P1/P2, Env, Steps, Repro, Logs, Attachments (traces/screenshots), Responsible team, SLA.

---

*Template: test-plan-template.md | Output: test_plan_taskflow.md*