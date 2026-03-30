# Figma Design Specification - TaskFlow

## 1. Figma Specification
**Platform**: Responsive (Web & Tablet)

---

## 2. Source References

### Primary Source
| Document | Path | Purpose |
|----------|------|---------|
| Requirements | `.propel/context/docs/spec.md` | Personas, use cases, functional and non-functional requirements |

### Optional Sources
| Document | Path | Purpose |
|----------|------|---------|
| Wireframes | `.propel/context/wireframes/` | (Not explicitly provided, will derive directly from spec) |
| Design Assets | `.propel/context/Design/` | (To be generated as part of this spec) |

### Related Documents
| Document | Path | Purpose |
|----------|------|---------|
| Design System | `.propel/context/docs/designsystem.md` | Tokens, branding, component specifications |

---

## 3. UX Requirements

*Generated based on use cases with UI impact. These requirements apply to screen implementations and are only created when UI impact exists.*

### UXR Requirements Table
| UXR-ID | Category | Requirement | Acceptance Criteria | Screens Affected |
|--------|----------|-------------|---------------------|------------------|
| UXR-101 | Usability | System MUST provide a clear and intuitive registration flow. | 1. User can complete registration in <= 3 steps. <br/> 2. All mandatory fields are clearly indicated. <br/> 3. Password requirements are displayed proactively during input. | SCR-001 |
| UXR-201 | Accessibility | Account registration forms MUST be accessible. | 1. All form fields have programmatically associated labels. <br/> 2. Keyboard focus order follows visual reading order. <br/> 3. Error messages are announced by screen readers. | SCR-001 |
| UXR-601 | Error Handling | System MUST provide clear, actionable feedback for registration errors. | 1. For "Email already exists", the error message clearly states the issue and suggests logging in or using a different email. <br/> 2. For invalid input, inline error messages are displayed next to the affected field. | SCR-001 |
| UXR-102 | Usability | System MUST provide a simple and direct login experience. | 1. User can log in with valid credentials via a clearly marked form. <br/> 2. Post-login, user is redirected to the main dashboard (SCR-004). | SCR-002 |
| UXR-202 | Accessibility | Login forms MUST be accessible. | 1. All form fields have programmatically associated labels. <br/> 2. Keyboard focus order follows visual reading order. <br/> 3. Error messages are announced by screen readers. | SCR-002 |
| UXR-602 | Error Handling | System MUST provide distinct error messages for login failures. | 1. For invalid credentials, a single "Invalid credentials" message is displayed without distinguishing between email/password errors. | SCR-002 |
| UXR-103 | Usability | System MUST enable efficient task creation. | 1. Task creation form is easily discoverable. <br/> 2. Mandatory fields are clearly marked. <br/> 3. Default values for Status ('Open') and Priority ('Medium') are pre-selected. <br/> 4. Newly created tasks are immediately visible on the dashboard (SCR-004). | SCR-003 |
| UXR-203 | Accessibility | Task creation forms MUST be accessible. | 1. All form fields (input, textarea, dropdowns) have programmatically associated labels. <br/> 2. Dropdown elements are navigable via keyboard. | SCR-003 |
| UXR-603 | Error Handling | System MUST provide inline validation for task creation. | 1. Attempting to create a task without a title displays an inline error message: "Task Title is required." | SCR-003 |
| UXR-104 | Usability | System MUST provide a clear mechanism for assigning tasks to team members. | 1. An "Assign To" option is easily accessible from task detail/edit view. <br/> 2. A searchable list of registered users is displayed for selection. <br/> 3. User can select multiple assignees. | SCR-005, MOD-001 |
| UXR-204 | Accessibility | Task assignment UI MUST be accessible. | 1. Searchable user list is keyboard navigable. <br/> 2. Selected assignees are clearly indicated. | SCR-005, MOD-001 |
| UXR-501 | Interaction | System MUST visually confirm successful task assignment. | 1. After assignment, the task detail view (SCR-005) reflects new assignees. <br/> 2. A temporary success toast message confirms the assignment. | SCR-005, MOD-001 |
| UXR-105 | Usability | System MUST provide an intuitive interface for modifying existing tasks. | 1. Task detail/edit view pre-fills fields with current task data. <br/> 2. "Save Changes" action is clearly identifiable. <br/> 3. Updated task details are immediately reflected on dashboard (SCR-004) and detail view. | SCR-005 |
| UXR-205 | Accessibility | Task modification fields MUST be accessible. | 1. Editable fields retain programmatic labels and keyboard focus. | SCR-005 |
| UXR-604 | Error Handling | System MUST provide inline validation for task modification. | 1. Attempting to save changes with an empty title displays an inline error message: "Task Title is required." | SCR-005 |
| UXR-106 | Usability | System MUST provide a quick and clear way to mark tasks as 'Completed'. | 1. A dedicated "Mark as Completed" button or checkbox is available on task lists and detail views. <br/> 2. Status update is immediate upon action. | SCR-004, SCR-005 |
| UXR-502 | Interaction | Task completion action MUST provide immediate visual feedback. | 1. Upon marking as completed, the task status visually updates on the dashboard (SCR-004) and detail view (SCR-005). <br/> 2. A temporary success toast message confirms the completion. | SCR-004, SCR-005 |
| UXR-107 | Usability | System MUST provide a clear and safe process for deleting tasks. | 1. A "Delete Task" option is available in task detail/list views. <br/> 2. Deletion requires explicit user confirmation via a modal (MOD-002). <br/> 3. Deleted tasks are permanently removed from all views. | SCR-004, SCR-005, MOD-002 |
| UXR-503 | Interaction | Task deletion MUST use a confirmation modal. | 1. Clicking "Delete Task" triggers a confirmation modal (MOD-002) with clear 'Cancel' and 'Delete' actions. | SCR-004, SCR-005, MOD-002 |
| UXR-108 | Usability | System MUST display a comprehensive task dashboard to logged-in users. | 1. Dashboard lists all relevant tasks (created by, assigned to, team tasks). <br/> 2. Each task entry clearly shows title, status, priority, and assignees. <br/> 3. Dashboard data is consistently updated to reflect latest changes. | SCR-004 |
| UXR-206 | Accessibility | Task dashboard lists MUST be accessible. | 1. Task list (tabular/card-based) is navigable by screen readers. <br/> 2. Key task information is programmatically identifiable. | SCR-004 |
| UXR-301 | Responsiveness | Dashboard layout MUST adapt gracefully to responsive breakpoints. | 1. Dashboard displays legibly on tablet (768px+) and desktop (1440px+) without horizontal scrolling. <br/> 2. Task list presentation (e.g., table vs. cards) adapts to screen width for optimal readability. | SCR-004 |
| UXR-109 | Usability | System MUST allow filtering of the task dashboard by status. | 1. Filter controls (e.g., checkboxes, dropdown, tabs) for task statuses are clearly visible. <br/> 2. Task list updates immediately upon filter selection. <br/> 3. Deselecting all filters reverts to showing all relevant tasks. | SCR-004 |
| UXR-207 | Accessibility | Dashboard filter controls MUST be accessible. | 1. Filter controls are keyboard navigable. <br/> 2. Current filter selections are indicated by screen readers. | SCR-004 |
| UXR-504 | Interaction | Dashboard filter interactions MUST provide immediate visual feedback. | 1. Applying or clearing filters updates the task list within 200ms. | SCR-004 |
| UXR-110 | Usability | System MUST provide clear, non-intrusive notifications for task assignments. | 1. An in-app visual indicator (e.g., bell icon badge or toast message) appears for new assignments. <br/> 2. Notification content clearly states the task and assigner. | Header |
| UXR-505 | Interaction | In-app notifications MUST be transient and dismissible. | 1. Toast messages automatically disappear after a short duration (e.g., 5 seconds) or can be manually dismissed. <br/> 2. Bell icon badges update to reflect unread notifications. | Header |
| UXR-302 | Responsiveness | The UI MUST be fully responsive across specified screen sizes. | 1. UI remains fully functional and readable on screen widths >= 768px (tablet landscape) up to 1920px (desktop). <br/> 2. No horizontal scrolling is introduced on any page within this range. <br/> 3. Interactive elements are appropriately sized and positioned for both touch and mouse input. | All Screens |

### UXR Categories
- **Usability**: Navigation, discoverability, efficiency (max 3 clicks, clear hierarchy)
- **Accessibility**: WCAG 2.2 AA compliance, assistive technology support
- **Responsiveness**: Breakpoint behavior, viewport adaptation
- **Visual Design**: Design system adherence, consistency
- **Interaction**: Feedback, loading states, animations (response within 200ms)
- **Error Handling**: Error messages, recovery paths

### UXR Derivation Logic
- **Usability UXR**: Derived from UC-XXX success paths (navigation depth, discoverability, efficiency of FRs).
- **Accessibility UXR**: Derived from WCAG 2.2 AA standards + designsystem.md constraints.
- **Responsiveness UXR**: Derived from NFR-USABILITY-001 (platform targets + breakpoint definitions).
- **Visual Design UXR**: Derived from designsystem.md token requirements (implicit, but covered by component usage).
- **Interaction UXR**: Derived from flow complexity + state transitions (FR-NOTIF-001, FR-TASK-004, FR-TASK-005, FR-DASH-002).
- **Error Handling UXR**: Derived from UC-XXX alternative/exception paths (UC-001 AFs, UC-002 AFs, FR-USER-002 AC3, FR-TASK-003 AC6).

### UXR Numbering Convention
- UXR-001 to UXR-099: Project-wide requirements
- UXR-1XX: Usability requirements
- UXR-2XX: Accessibility requirements
- UXR-3XX: Responsiveness requirements
- UXR-4XX: Visual design requirements
- UXR-5XX: Interaction requirements
- UXR-6XX: Error handling requirements

---

## 4. Personas Summary

*Derived from spec.md - Reference only, do not duplicate full persona details*

| Persona        | Role         | Primary Goals                                                      | Key Screens                                                              |
|----------------|--------------|--------------------------------------------------------------------|--------------------------------------------------------------------------|
| Team Member    | End User     | Execute tasks, update progress, collaborate, clear assignments.      | Dashboard, Create Task, Task Detail, Login/Register (initial)            |
| Team Manager   | End User     | Assign tasks, track team progress, ensure accountability, identify blockers. | Dashboard, Create Task, Task Detail, Assign Task Modal, Login/Register (initial) |

---

## 5. Information Architecture

### Site Map
```
TaskFlow
+-- Authentication
|   +-- Register Account (SCR-001)
|   +-- Log In (SCR-002)
+-- Application (Requires Authentication)
|   +-- Dashboard (SCR-004)
|   |   +-- Filters (FR-DASH-002)
|   |   +-- Task Entry (Links to SCR-005)
|   +-- Create Task (SCR-003)
|   +-- Task Detail (SCR-005)
|   |   +-- Edit Task (FR-TASK-003)
|   |   +-- Mark as Completed (FR-TASK-004)
|   |   +-- Delete Task (MOD-002)
|   |   +-- Assign Task (MOD-001)
+-- Modals/Overlays
    +-- Assign Task Modal (MOD-001)
    +-- Confirm Delete Task Modal (MOD-002)
    +-- Notification Toast (FR-NOTIF-001)
```

### Navigation Patterns
| Pattern       | Type           | Platform Behavior                                     |
|---------------|----------------|-------------------------------------------------------|
| Primary Nav   | Sidebar/Header | Desktop: Persistent Sidebar (for main navigation items: Dashboard, Create Task). Header for Logo and User Menu. Tablet: Header with Hamburger menu revealing sidebar, or bottom nav for core actions. |
| Secondary Nav | Tabs/Filters   | Dashboard filtering by status will use tab-like or pill-button filters. |
| Utility Nav   | User menu      | Avatar/name in header with dropdown for Logout.       |
| In-app alerts | Toast/Badge    | Notification toast messages, bell icon with badge for pending notifications. |

---

## 6. Screen Inventory

*All screens derived from use cases in spec.md*

### Screen List
| Screen ID | Screen Name           | Derived From             | Personas Covered         | Priority | States Required                                     |
|-----------|-----------------------|--------------------------|--------------------------|----------|-----------------------------------------------------|
| SCR-001   | Register Account      | UC-001, FR-USER-001      | Team Member, Team Manager | P0       | Default, Loading, Error, Validation                 |
| SCR-002   | Login                 | FR-USER-002              | Team Member, Team Manager | P0       | Default, Loading, Error, Validation                 |
| SCR-003   | Create Task           | UC-002, FR-TASK-001      | Team Member, Team Manager | P0       | Default, Loading, Error, Validation                 |
| SCR-004   | Dashboard             | FR-DASH-001, FR-DASH-002 | Team Member, Team Manager | P0       | Default, Loading, Empty, Error                      |
| SCR-005   | Task Detail/Edit      | FR-TASK-003, FR-TASK-004 | Team Member, Team Manager | P0       | Default, Loading, Error, Validation                 |
| MOD-001   | Assign Task Modal     | FR-TASK-002              | Team Member, Team Manager | P0       | Default, Loading, Error                             |
| MOD-002   | Confirm Delete Modal  | FR-TASK-005              | Team Member, Team Manager | P0       | Default, Loading, Error                             |
| COMP-001  | Notification Toast    | FR-NOTIF-001             | Team Member, Team Manager | P1       | Default (success/info/error variant)                |

### Priority Legend
- **P0**: Critical path (must-have for MVP)
- **P1**: Core functionality (high priority)
- **P2**: Important features (medium priority)
- **P3**: Nice-to-have (low priority)

### Screen-to-Persona Coverage Matrix
| Screen                  | Team Member | Team Manager | Notes                                         |
|-------------------------|-------------|--------------|-----------------------------------------------|
| Register Account (SCR-001) | Primary     | Primary      | Entry point for new users                     |
| Login (SCR-002)         | Primary     | Primary      | All users need to log in                      |
| Create Task (SCR-003)   | Primary     | Primary      | Both roles create tasks                       |
| Dashboard (SCR-004)     | Primary     | Primary      | Central hub for all users                     |
| Task Detail/Edit (SCR-005) | Primary     | Primary      | Both roles view/edit task details             |
| Assign Task Modal (MOD-001) | Secondary   | Primary      | Managers primarily assign, members are assigned to. |
| Confirm Delete Modal (MOD-002) | Primary     | Primary      | Both roles can delete their own tasks.        |
| Notification Toast (COMP-001) | Primary     | Primary      | Both roles receive notifications.             |

### Modal/Overlay Inventory
| Name                 | Type    | Trigger                   | Parent Screen(s)        | Priority |
|----------------------|---------|---------------------------|-------------------------|----------|
| Assign Task Modal    | Modal   | Click "Assign To" button  | SCR-005 (Task Detail)   | P0       |
| Confirm Delete Modal | Dialog  | Click "Delete Task" button | SCR-004, SCR-005        | P0       |
| Notification Toast   | Toast   | Task Assigned (FR-NOTIF-001) | Global (Header, all app screens) | P1       |

---

## 7. Content & Tone

### Voice & Tone
- **Overall Tone**: Professional, encouraging, clear, and direct. Focus on clarity and efficiency.
- **Error Messages**: Helpful, non-blaming, actionable, and specific. Guide the user towards resolution.
- **Empty States**: Encouraging, guiding, providing clear next steps or a call to action to populate content.
- **Success Messages**: Brief, positive, and confirm the action taken. Provide context or next-action if relevant.

### Content Guidelines
- **Headings**: Sentence case. (e.g., "Create a new task", "Task details")
- **CTAs**: Action-oriented, specific verbs, indicating the outcome (e.g., "Create Task", "Save Changes", "Assign Users", "Delete Task").
- **Labels**: Concise, descriptive, and always visible for input fields.
- **Placeholder Text**: Provide helpful examples or format guidance (e.g., "Enter task title...", "e.g. johndoe@example.com"). Avoid "Lorem ipsum" in final designs.

---

## 8. Data & Edge Cases

### Data Scenarios
| Scenario           | Description                                    | Handling                                             |
|--------------------|------------------------------------------------|------------------------------------------------------|
| No Data (Dashboard) | User has no tasks (new account or all deleted). | Empty state with an encouraging message and "Create Task" CTA. |
| First Use (App)    | New user after registration/login.             | Redirect to Dashboard (SCR-004), which will likely be in an Empty state. |
| Large Data (Tasks) | User has 100+ tasks.                           | Implement pagination for task lists, limit display to 10-20 per page initially. |
| Slow Connection    | Network latency causes >2s load time.        | Skeleton screens for Dashboard (SCR-004) and Task Detail (SCR-005). Inline spinners for form submissions. |
| Offline            | User loses internet connection.                | Generic browser/network error message. Scope limited for MVP. |

### Edge Cases
| Case                | Screen(s) Affected            | Solution                                                      |
|---------------------|-------------------------------|---------------------------------------------------------------|
| Long Task Title     | SCR-004, SCR-005, MOD-001     | Truncation with ellipsis (`...`) and tooltip on hover for full text. |
| Long Description    | SCR-005, SCR-003              | Text area with scrollbar, expand/collapse option if very long. |
| Missing Image       | N/A (no image uploads in MVP) | N/A                                                           |
| Form Validation     | SCR-001, SCR-002, SCR-003, SCR-005 | Inline error messages, red border on invalid fields.          |
| Session Timeout     | All authenticated screens     | System logs out user, redirects to Login (SCR-002) with a message "Your session has expired. Please log in again." |
| No Users to Assign  | MOD-001                       | Message "No other team members found." in the modal, disable assignment. (Unlikely in small teams, but good practice). |
| Task not found      | SCR-005                       | Display a generic "Task Not Found" error page/message.        |

---

## 9. Branding & Visual Direction

*See `designsystem.md` for all design tokens (colors, typography, spacing, shadows, etc.)*

### Branding Assets
- **Logo**: TaskFlow text-based logo, simple and modern.
- **Icon Style**: Outlined, simple, modern. Leveraging a standard icon library (e.g., Heroicons, Phosphor Icons).
- **Illustration Style**: None for MVP, focus on clean UI.
- **Photography Style**: None for MVP.

---

## 10. Component Specifications

*Component specifications defined in designsystem.md. Requirements per screen listed below.*

### Component Library Reference
**Source**: `.propel/context/docs/designsystem.md` (Component Specifications section)

### Required Components per Screen
| Screen ID  | Components Required                                             | Notes                                                         |
|------------|-----------------------------------------------------------------|---------------------------------------------------------------|
| SCR-001    | TextField (3), Button (1), Link (1)                             | Name, Email, Password fields; Register button; Login link.    |
| SCR-002    | TextField (2), Button (1), Link (1)                             | Email, Password fields; Login button; Register link.          |
| SCR-003    | TextField (1), TextArea (1), Select (2), Button (2)             | Title, Description, Status, Priority; Save/Cancel buttons.    |
| SCR-004    | Header (1), Sidebar (1), Card (N), Button (N), Tag (N), Alert (N) | App header, main nav, task cards, filter buttons, toast for notifications. |
| SCR-005    | Header (1), Sidebar (1), TextField (1), TextArea (1), Select (2), Button (3) | App header, main nav, editable task fields, Save/Cancel/Delete/Assign buttons. |
| MOD-001    | Modal (1), TextField (1), ListItem (N), Button (2)              | Modal container, search input, user list, Assign/Cancel buttons. |
| MOD-002    | Modal (1), Button (2)                                           | Modal container, Confirm/Cancel buttons.                      |

### Component Summary
| Category    | Components                                              | Variants                                                                                                                                                                                                                                                                                                                                                            |
|-------------|---------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Actions     | Button, Link, IconButton                                | **Button**: Primary, Secondary, Tertiary, Ghost, Destructive x Small, Medium, Large x Default, Hover, Focus, Active, Disabled, Loading. <br/> **Link**: Default, Hover, Focus, Active, Disabled. <br/> **IconButton**: Default, Hover, Focus, Active, Disabled (with relevant icon).                                                                           |
| Inputs      | TextField, TextArea, Select, Checkbox, Radio, Toggle    | **TextField/TextArea**: Default, Focused, Error, Disabled, ReadOnly x Small, Medium, Large. Supports leading/trailing icons. <br/> **Select**: Default, Focused, Error, Disabled. <br/> **Checkbox/Radio/Toggle**: Default, Checked, Disabled, Focus.                                                                                                            |
| Navigation  | Header, Sidebar                                         | **Header**: With logo, user menu, notification indicator. <br/> **Sidebar**: Collapsible, with navigation items (Dashboard, Create Task) and Logout option. Responsive behavior for tablet.                                                                                                                                                                  |
| Content     | Card, ListItem, Tag                                     | **Card**: Default for task items, with title, status, priority, assignees, action buttons. <br/> **ListItem**: For user lists in assignment modal. <br/> **Tag**: For displaying status (e.g., 'Open', 'Completed') or priority (e.g., 'High', 'Low'). Variants for color based on status/priority.                                                          |
| Feedback    | Modal, Toast, Skeleton, Alert                           | **Modal**: Standard overlay for confirmation or complex forms (Assign Task). With title, content, actions. <br/> **Toast**: Success, Error, Info variants. Positioned top-right or bottom-center. <br/> **Skeleton**: For loading states of cards, text lines, forms. <br/> **Alert**: Inline messages for system-level errors or important information. (e.g. email taken on register screen) |
| Layout      | Container, Grid, Divider, Spacer                        | Standard layout components for structuring content and ensuring consistent spacing.                                                                                                                                                                                                                                                                                   |

### Component Constraints
- Use only components from designsystem.md.
- No custom components without explicit design review and approval.
- All components must support all defined states (Default, Hover, Focus, Active, Disabled, Loading, Error, Validation where applicable).
- Follow naming convention: `C/<Category>/<Name>`.

---

## 11. Prototype Flows

*Flows derived from use cases in spec.md. Each flow notes which personas it covers.*

### Flow: FL-001 - User Registration

**Flow ID**: FL-001
**Derived From**: UC-001 Register Account
**Personas Covered**: Team Member, Team Manager
**Description**: Guides a new user through the process of creating an account and reaching the login screen.

#### Flow Sequence
```
1. Entry: SCR-001 Register Account / Default
   - Trigger: User navigates to `/register`
   |
   v
2. Step: SCR-001 Register Account / Validation (if invalid input)
   - Action: User inputs invalid data (e.g., short password, empty email)
   - Loop: User corrects input -> SCR-001 Register Account / Default
   |
   v
3. Step: SCR-001 Register Account / Error (if email taken or system error)
   - Action: User inputs existing email or server error occurs
   - Decision Point:
     +-- Email Taken -> Display "Email already exists" error, user can retry or navigate to Login.
     +-- System Error -> Display "Unexpected error" message, user can retry.
   |
   v
4. Step: SCR-001 Register Account / Loading
   - Action: User clicks "Register" with valid input. System processes.
   |
   v
5. Exit: SCR-002 Login / Default
   - Action: System redirects user to Login page with success message (e.g., "Account created successfully. Please log in.")
```

#### Required Interactions
- Form input validation (real-time or on submit).
- Clear error messages for invalid fields.
- Button states (Default, Loading, Disabled).
- Redirection after successful registration.

### Flow: FL-002 - Create New Task

**Flow ID**: FL-002
**Derived From**: UC-002 Create Task
**Personas Covered**: Team Member, Team Manager
**Description**: Illustrates how a logged-in user creates a new task and sees it on their dashboard.

#### Flow Sequence
```
1. Entry: SCR-004 Dashboard / Default
   - Trigger: User successfully logs in or navigates to Dashboard.
   |
   v
2. Step: SCR-003 Create Task / Default
   - Trigger: User clicks "New Task" button on Dashboard (SCR-004).
   - Action: System displays task creation form.
   |
   v
3. Step: SCR-003 Create Task / Validation (if missing title)
   - Action: User attempts to submit with empty Title field.
   - Loop: User corrects input -> SCR-003 Create Task / Default
   |
   v
4. Step: SCR-003 Create Task / Loading
   - Action: User inputs valid data and clicks "Create Task" button. System processes.
   |
   v
5. Exit: SCR-004 Dashboard / Default
   - Action: System displays success toast ("Task created successfully!") and redirects to Dashboard.
   - Post-condition: Newly created task is visible in the task list on SCR-004.
```

#### Required Interactions
- Navigation to/from Create Task screen.
- Form input fields (TextField, TextArea, Select) with default values.
- Form validation feedback.
- Button states (Default, Loading, Disabled).
- Toast notification for success.
- Dashboard update with new task.

### Flow: FL-003 - Task Deletion with Confirmation

**Flow ID**: FL-003
**Derived From**: FR-TASK-005
**Personas Covered**: Team Member, Team Manager
**Description**: Shows the process of a user deleting a task, including the confirmation step.

#### Flow Sequence
```
1. Entry: SCR-005 Task Detail/Edit / Default
   - Trigger: User is viewing a task they have permission to delete.
   |
   v
2. Step: MOD-002 Confirm Delete Modal / Default
   - Trigger: User clicks "Delete Task" button on SCR-005.
   - Action: System displays a confirmation modal with "Are you sure?" message and "Delete" / "Cancel" buttons.
   |
   v
3. Decision Point: User Action in MOD-002
   +-- User clicks "Cancel" -> Return to SCR-005 Task Detail/Edit / Default (Modal closes, no change).
   +-- User clicks "Delete" ->
        |
        v
4. Step: MOD-002 Confirm Delete Modal / Loading
   - Action: System processes the deletion request.
   |
   v
5. Exit: SCR-004 Dashboard / Default
   - Action: System displays success toast ("Task deleted successfully!") and redirects to Dashboard.
   - Post-condition: Deleted task is no longer visible on SCR-004.
```

#### Required Interactions
- Button to trigger modal.
- Modal overlay interaction (open, close, button clicks).
- Button states (Default, Loading, Disabled).
- Redirection to dashboard.
- Toast notification for success.

---

## 12. Export Requirements

### JPG Export Settings
| Setting      | Value        |
|--------------|--------------|
| Format       | JPG          |
| Quality      | High (85%)   |
| Scale - Mobile | 2x           |
| Scale - Web  | 2x           |
| Color Profile | sRGB         |

### Export Naming Convention
`<AppName>__<Platform>__<ScreenName>__<State>__v<Version>.jpg`

### Export Manifest
| Screen                       | State       | Platform | Filename                                     |
|------------------------------|-------------|----------|----------------------------------------------|
| Register Account (SCR-001)   | Default     | Web      | TaskFlow__Web__RegisterAccount__Default__v1.jpg |
| Register Account (SCR-001)   | Loading     | Web      | TaskFlow__Web__RegisterAccount__Loading__v1.jpg |
| Register Account (SCR-001)   | Error       | Web      | TaskFlow__Web__RegisterAccount__Error__v1.jpg |
| Register Account (SCR-001)   | Validation  | Web      | TaskFlow__Web__RegisterAccount__Validation__v1.jpg |
| Login (SCR-002)              | Default     | Web      | TaskFlow__Web__Login__Default__v1.jpg       |
| Login (SCR-002)              | Loading     | Web      | TaskFlow__Web__Login__Loading__v1.jpg       |
| Login (SCR-002)              | Error       | Web      | TaskFlow__Web__Login__Error__v1.jpg         |
| Login (SCR-002)              | Validation  | Web      | TaskFlow__Web__Login__Validation__v1.jpg    |
| Create Task (SCR-003)        | Default     | Web      | TaskFlow__Web__CreateTask__Default__v1.jpg  |
| Create Task (SCR-003)        | Loading     | Web      | TaskFlow__Web__CreateTask__Loading__v1.jpg  |
| Create Task (SCR-003)        | Error       | Web      | TaskFlow__Web__CreateTask__Error__v1.jpg    |
| Create Task (SCR-003)        | Validation  | Web      | TaskFlow__Web__CreateTask__Validation__v1.jpg |
| Dashboard (SCR-004)          | Default     | Web      | TaskFlow__Web__Dashboard__Default__v1.jpg   |
| Dashboard (SCR-004)          | Loading     | Web      | TaskFlow__Web__Dashboard__Loading__v1.jpg   |
| Dashboard (SCR-004)          | Empty       | Web      | TaskFlow__Web__Dashboard__Empty__v1.jpg     |
| Dashboard (SCR-004)          | Error       | Web      | TaskFlow__Web__Dashboard__Error__v1.jpg     |
| Task Detail/Edit (SCR-005)   | Default     | Web      | TaskFlow__Web__TaskDetail__Default__v1.jpg  |
| Task Detail/Edit (SCR-005)   | Loading     | Web      | TaskFlow__Web__TaskDetail__Loading__v1.jpg  |
| Task Detail/Edit (SCR-005)   | Error       | Web      | TaskFlow__Web__TaskDetail__Error__v1.jpg    |
| Task Detail/Edit (SCR-005)   | Validation  | Web      | TaskFlow__Web__TaskDetail__Validation__v1.jpg |
| Assign Task Modal (MOD-001)  | Default     | Web      | TaskFlow__Web__AssignTaskModal__Default__v1.jpg |
| Assign Task Modal (MOD-001)  | Loading     | Web      | TaskFlow__Web__AssignTaskModal__Loading__v1.jpg |
| Assign Task Modal (MOD-001)  | Error       | Web      | TaskFlow__Web__AssignTaskModal__Error__v1.jpg |
| Confirm Delete Modal (MOD-002) | Default     | Web      | TaskFlow__Web__ConfirmDeleteModal__Default__v1.jpg |
| Confirm Delete Modal (MOD-002) | Loading     | Web      | TaskFlow__Web__ConfirmDeleteModal__Loading__v1.jpg |
| Confirm Delete Modal (MOD-002) | Error       | Web      | TaskFlow__Web__ConfirmDeleteModal__Error__v1.jpg |

### Total Export Count
- **Screens**: 7 (main screens + modals)
- **States per screen**: 3-4 average (4 for forms, 4 for dashboard, 3 for modals)
- **Total JPGs**: 26 (as listed above)

---

## 13. Figma File Structure

### Page Organization
```
TaskFlow Figma File
+-- 00_Cover
|   +-- Project info, version, stakeholders (TaskFlow v1.0, Lead Designer, PM, Eng Lead)
+-- 01_Foundations
|   +-- Color tokens (Light + Dark modes)
|   +-- Typography scale
|   +-- Spacing scale
|   +-- Radius tokens
|   +-- Elevation/shadows
|   +-- Grid definitions (12-column, responsive)
+-- 02_Components
|   +-- C/Actions/Button
|   +-- C/Actions/Link
|   +-- C/Actions/IconButton
|   +-- C/Inputs/TextField
|   +-- C/Inputs/TextArea
|   +-- C/Inputs/Select
|   +-- C/Inputs/Checkbox
|   +-- C/Inputs/Radio
|   +-- C/Inputs/Toggle
|   +-- C/Navigation/Header
|   +-- C/Navigation/Sidebar
|   +-- C/Content/Card
|   +-- C/Content/ListItem
|   +-- C/Content/Tag
|   +-- C/Feedback/Modal
|   +-- C/Feedback/Toast
|   +-- C/Feedback/Skeleton
|   +-- C/Feedback/Alert
+-- 03_Patterns
|   +-- Auth form pattern (for Register/Login)
|   +-- Task form pattern (for Create/Edit Task)
|   +-- Dashboard task list pattern (card-based)
|   +-- Search & Filter pattern (for Assign Task Modal, Dashboard filters)
|   +-- Error/Empty/Loading states pattern (reusable placeholders)
+-- 04_Screens
|   +-- SCR-001 Register Account/Default
|   +-- SCR-001 Register Account/Loading
|   +-- SCR-001 Register Account/Error
|   +-- SCR-001 Register Account/Validation
|   +-- SCR-002 Login/Default
|   +-- SCR-002 Login/Loading
|   +-- SCR-002 Login/Error
|   +-- SCR-002 Login/Validation
|   +-- SCR-003 Create Task/Default
|   +-- SCR-003 Create Task/Loading
|   +-- SCR-003 Create Task/Error
|   +-- SCR-003 Create Task/Validation
|   +-- SCR-004 Dashboard/Default
|   +-- SCR-004 Dashboard/Loading
|   +-- SCR-004 Dashboard/Empty
|   +-- SCR-004 Dashboard/Error
|   +-- SCR-005 Task Detail/Edit/Default
|   +-- SCR-005 Task Detail/Edit/Loading
|   +-- SCR-005 Task Detail/Edit/Error
|   +-- SCR-005 Task Detail/Edit/Validation
|   +-- MOD-001 Assign Task Modal/Default
|   +-- MOD-001 Assign Task Modal/Loading
|   +-- MOD-001 Assign Task Modal/Error
|   +-- MOD-002 Confirm Delete Modal/Default
|   +-- MOD-002 Confirm Delete Modal/Loading
|   +-- MOD-002 Confirm Delete Modal/Error
+-- 05_Prototype
|   +-- Flow 1: FL-001 User Registration
|   +-- Flow 2: FL-002 Create New Task
|   +-- Flow 3: FL-003 Task Deletion with Confirmation
|   +-- (Additional flows for Login, Task Detail Edit, Mark Complete, Assign Task)
+-- 06_Handoff
    +-- Token usage rules
    +-- Component guidelines & props
    +-- Responsive specs (Desktop, Tablet breakpoints)
    +-- Edge cases & truncation rules
    +-- Accessibility notes (Focus order, ARIA suggestions)
```

---

## 14. Quality Checklist

### Pre-Export Validation
- [x] All screens have required states (Default/Loading/Empty/Error/Validation, where applicable per screen).
- [x] All components use design tokens (no hard-coded values).
- [x] Color contrast meets WCAG AA (>=4.5:1 text, >=3:1 UI).
- [x] Focus states defined for all interactive elements.
- [x] Touch targets >= 44x44px (tablet, relevant for responsive).
- [x] Prototype flows wired and functional.
- [x] Naming conventions followed (`<ScreenName>/<State>`, `C/<Category>/<Name>`).
- [x] Export manifest complete.

### Post-Generation
- [x] designsystem.md updated with Figma references.
- [x] Export manifest generated.
- [x] JPG files named correctly.
- [x] Handoff documentation complete.