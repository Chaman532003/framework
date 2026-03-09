# Business Requirements Document (BRD)

## Project Title
TaskFlow – Simple Team Task Management System

---

# 1. Goal

The goal of TaskFlow is to provide a lightweight web application where small teams can create, assign, and track tasks efficiently.

The system should improve collaboration and task visibility within teams while keeping the interface simple and easy to use.

---

# 2. Business Objectives

- Improve team productivity by organizing tasks in one platform
- Allow managers to track task progress easily
- Provide clear accountability through task assignments
- Reduce reliance on email or spreadsheets for task tracking

---

# 3. Scope

## In Scope

- User registration and login
- Task creation and assignment
- Task status tracking
- Task editing and deletion
- Dashboard showing all tasks
- Simple notification system

## Out of Scope

- Advanced project analytics
- AI-based task suggestions
- Mobile applications
- Integration with external project management tools

---

# 4. Stakeholders

| Role | Responsibility |
|-----|---------------|
| Product Owner | Defines product requirements |
| Development Team | Implements the system |
| Project Manager | Manages delivery timeline |
| End Users | Use the system to manage tasks |

---

# 5. Functional Requirements (FR)

| ID | Requirement |
|---|-------------|
| FR1 | Users must be able to register and create an account |
| FR2 | Users must be able to log in securely |
| FR3 | Users must be able to create new tasks |
| FR4 | Users must be able to assign tasks to team members |
| FR5 | Users must be able to edit existing tasks |
| FR6 | Users must be able to mark tasks as completed |
| FR7 | Users must be able to delete tasks |
| FR8 | The system must display a dashboard of all tasks |
| FR9 | Users must be able to filter tasks by status |
| FR10 | Users must receive notifications when tasks are assigned |

---

# 6. Non-Functional Requirements (NFR)

| ID | Requirement |
|---|-------------|
| NFR1 | System should support at least 500 concurrent users |
| NFR2 | API response time should be under 2 seconds |
| NFR3 | System uptime should be at least 99.5% |
| NFR4 | User passwords must be securely hashed |
| NFR5 | Application must use HTTPS for all communication |
| NFR6 | UI must be responsive and usable on desktop and tablet |

---

# 7. Data Requirements (DR)

| Entity | Description |
|------|-------------|
| User | Stores user account information |
| Task | Stores task details |
| Assignment | Maps tasks to users |

### User Table

- user_id
- name
- email
- password_hash
- created_at

### Task Table

- task_id
- title
- description
- status
- priority
- created_by
- created_at

### Assignment Table

- assignment_id
- task_id
- user_id
- assigned_at

---

# 8. Technology Stack

## Frontend
- React
- Tailwind CSS

## Backend
- FastAPI (Python)

## Database
- PostgreSQL

## Authentication
- JWT-based authentication

## Deployment
- Docker
- AWS or Azure Cloud

---

# 9. Assumptions

- Users have basic familiarity with web applications
- Teams will consist of fewer than 50 members
- Internet connectivity is available

---

# 10. Constraints

- Initial release must be completed within 3 months
- The system should minimize operational costs
- Only open-source technologies should be used where possible

---

# 11. Success Criteria

The project will be considered successful if:

- Users can create and manage tasks easily
- Task completion rates improve within teams
- System adoption reaches at least 80% of the target users