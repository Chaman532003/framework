# Figma Design Specification - TaskFlow

## 1. Figma Specification
**Platform**: Responsive (Web & Tablet)

---

## 2. Source References

### Primary Source
| Document | Path | Purpose |
|----------|------|---------|
| Requirements | `.propel/context/docs/spec.md` | Personas, use cases, epics with UI impact flags |

### Optional Sources
| Document | Path | Purpose |
|----------|------|---------|
| Wireframes | `.propel/context/wireframes/` | Entity understanding, content structure |
| Design Assets | `.propel/context/Design/` | Visual references from spec.md epics |

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
| UXR-101 | Usability | System MUST provide a clear and intuitive registration process. | User can complete registration successfully within 3 steps. All form fields have clear labels and placeholder text. | SCR-001 |
| UXR-102 | Usability | System MUST enable efficient login and immediate access to dashboard. | User can log in in a single step (after initial registration). Upon successful login, user is redirected to the main dashboard (SCR-003) within 2 seconds. | SCR-002, SCR-003 |
| UXR-103 | Usability | System MUST provide an easy-to-use form for creating new tasks. | The 'Create Task' form fields are clearly labeled, include helpful placeholder/tooltip text, and mandatory fields are visibly indicated. Default values are pre-selected for status and priority. | SCR-004 |
| UXR-104 | Usability | System MUST provide clear visibility of all relevant tasks on the dashboard. | Each task entry on SCR-003 clearly displays title, status, priority, and assignee(s) at a glance. | SCR-003 |
| UXR-105 | Usability | System MUST allow users to filter tasks on the dashboard easily. | Filter controls for task status are readily accessible on SCR-003, and applying/clearing filters updates the task list within 1 second. | SCR-003 |
| UXR-106 | Usability | System MUST provide a straightforward way to manage individual tasks. | Users can access task details for editing, assigning, updating status, and deleting from the dashboard or a dedicated task view (SCR-005). | SCR-003, SCR-005 |
| UXR-201 | Accessibility | All interactive elements MUST meet WCAG 2.2 AA contrast standards. | Text contrast >= 4.5:1 for normal text and >= 3:1 for large text. UI component contrast >= 3:1. | ALL |
| UXR-202 | Accessibility | All interactive elements MUST provide clear and persistent focus indicators. | Focus states are visibly distinct (e.g., 2px outline offset) for buttons, input fields, links, and filters. | ALL |
| UXR-203 | Accessibility | All forms MUST have programmatically associated labels for inputs. | Each input field (`TextField`, `Select`) on forms (SCR-001, SCR-002, SCR-004, SCR-005) uses `<label for>` or `aria-labelledby`. | SCR-001, SCR-002, SCR-004, SCR-005 |
| UXR-204 | Accessibility | The UI MUST be fully keyboard navigable and follow a logical tab order. | Users can navigate through all interactive elements using Tab/Shift+Tab and activate them with Enter/Space. Tab order follows visual reading order. | ALL |
| UXR-205 | Accessibility | All images conveying information MUST have descriptive alt text. | Informative images (e.g., empty state illustrations) include meaningful `alt` attributes. Decorative images have `alt=""` or `aria-hidden="true"`. | SCR-003 |
| UXR-301 | Responsiveness | The UI MUST adapt gracefully to tablet and desktop screen sizes (768px and above). | Layouts, font sizes, and component dimensions adjust appropriately for widths 768px, 1024px, and 1440px without horizontal scrolling. | ALL |
| UXR-401 | Visual Design | All UI elements MUST adhere to the defined Design System tokens. | All colors, typography, spacing, and component styles match values in `designsystem.md`. | ALL |
| UXR-402 | Visual Design | The overall visual design MUST be consistent across all screens. | Components, layout patterns, and visual feedback mechanisms (e.g., modals, toasts) are visually uniform throughout the application. | ALL |
| UXR-501 | Interaction | System MUST provide clear visual feedback during loading states. | Skeleton screens or spinners are displayed for content areas loading for more than 300ms, maintaining layout structure. | ALL |
| UXR-502 | Interaction | System MUST provide immediate feedback for form submissions and actions. | Success/error messages (e.g., Toast notifications) are displayed upon form submission or critical actions (e.g., task creation, deletion). | SCR-001, SCR-002, SCR-004, SCR-005, SCR-003 |
| UXR-503 | Interaction | System MUST provide notifications for task assignments. | A clear visual indicator (e.g., an in-app toast or badge on a notification icon) informs the user of a new task assignment. | Global (Header), SCR-003 |
| UXR-601 | Error Handling | All form validation errors MUST be displayed inline and be actionable. | When validation fails (UC-001 AF-1.2, UC-002 AF-2.1), an error message appears directly below the problematic field, and the field is visually highlighted. | SCR-001, SCR-002, SCR-004, SCR-005 |
| UXR-602 | Error Handling | System MUST display clear and concise error messages for system failures. | For unexpected system errors (UC-001 AF-1.3, UC-002 AF-2.2), a user-friendly error message is displayed (e.g., "An unexpected error occurred. Please try again.") with potential retry options. | ALL (Error states) |
| UXR-603 | Error Handling | Destructive actions MUST require user confirmation. | Deleting a task (FR-TASK-005) or similar irreversible actions trigger a modal confirmation dialog before proceeding. | SCR-005, SCR-003 (via context menu) |

### UXR Derivation Logic
- **Usability UXR**: Derived from UC-001 (Register), UC-002 (Create Task), FR-DASH-001 (Dashboard), FR-DASH-002 (Filter), FR-TASK-003 (Modify), FR-TASK-005 (Delete).
- **Accessibility UXR**: Derived from WCAG 2.2 AA standards and NFR-USABILITY-001.
- **Responsiveness UXR**: Derived from NFR-USABILITY-001 for tablet and desktop targets.
- **Visual Design UXR**: Derived from `designsystem.md` adherence.
- **Interaction UXR**: Derived from UC success/alternative flows, FR-NOTIF-001, and general UX best practices for feedback.
- **Error Handling UXR**: Derived from UC-001/AF-1.1, AF-1.2, AF-1.3, UC-002/AF-2.1, AF-2.2, FR-TASK-005.

---

## 4. Personas Summary

*Derived from spec.md - Reference only, do not duplicate full persona details*

| Persona | Role | Primary Goals | Key Screens |
|---------|------|---------------|-------------|
| Team Member | Execute tasks, collaborate, update progress. | Clear assignments, easy task updates, simple interface. | Task Dashboard (SCR-003), Create Task (SCR-004), Task Detail/Edit (SCR-005) |
| Team Manager | Assign tasks, track progress, ensure accountability. | Visibility of team tasks, easy assignment, overview of project status. | Task Dashboard (SCR-003), Create Task (SCR-004), Task Detail/Edit (SCR-005) |

---

## 5. Information Architecture

### Site Map
```
TaskFlow
+-- Authentication
|   +-- Register Account (SCR-001)
|   +-- Log In (SCR-002)
+-- Main Application
    +-- Task Dashboard (SCR-003)
    |   +-- Create Task (SCR-004)
    |   +-- Task Detail / Edit (SCR-005)
    |       +-- Assign Task (Modal/Inline)
    |       +-- Delete Confirmation (Modal)
    +-- User Profile / Settings (Future Scope)
```

### Navigation Patterns
| Pattern | Type | Platform Behavior |
|---------|------|-------------------|
| Primary Nav | Header | Persistent header with Logo, "Create Task" button, User Menu, and potentially a notification indicator. Always visible on desktop and tablet. |
| Utility Nav | User Menu | Dropdown from user avatar/name in header, containing Logout and potentially Profile (future). |
| Contextual | Buttons / Links | Within dashboard and task detail for specific actions (e.g., Edit, Assign, Mark Complete, Delete). |
| Global Filters | Inline | Filter controls for task status prominently displayed on the Task Dashboard (SCR-003). |

---

## 6. Screen Inventory

*All screens derived from use cases in spec.md*

### Screen List
| Screen ID | Screen Name | Derived From | Personas Covered | Priority | States Required |
|-----------|-------------|--------------|------------------|----------|-----------------|
| SCR-001 | Register Account | UC-001 | Team Member, Team Manager | P0 | Default, Loading, Error, Validation |
| SCR-002 | Log In | FR-USER-002 | Team Member, Team Manager | P0 | Default, Loading, Error, Validation |
| SCR-003 | Task Dashboard | FR-DASH-001, FR-DASH-002, FR-NOTIF-001 | Team Member, Team Manager | P0 | Default, Loading, Empty, Error |
| SCR-004 | Create Task Form | UC-002, FR-TASK-001 | Team Member, Team Manager | P0 | Default, Loading, Error, Validation |
| SCR-005 | Task Detail / Edit | FR-TASK-002, FR-TASK-003, FR-TASK-004, FR-TASK-005 | Team Member, Team Manager | P0 | Default, Loading, Error, Validation |

### Priority Legend
- **P0**: Critical path (must-have for MVP)
- **P1**: Core functionality (high priority)
- **P2**: Important features (medium priority)
- **P3**: Nice-to-have (low priority)

### Screen-to-Persona Coverage Matrix
| Screen | Team Member | Team Manager | Notes |
|--------|-------------|--------------|-------|
| SCR-001 | Primary | Primary | Entry point for new users |
| SCR-002 | Primary | Primary | Entry point for existing users |
| SCR-003 | Primary | Primary | Central task management hub |
| SCR-004 | Primary | Primary | Essential for creating work |
| SCR-005 | Primary | Primary | Detailed task management |

### Modal/Overlay Inventory
| Name | Type | Trigger | Parent Screen(s) | Priority |
|------|------|---------|-----------------|----------|
| Confirmation Modal | Dialog | Delete Task action (FR-TASK-005) | SCR-003 (context menu), SCR-005 | P0 |
| Assign Task Modal | Modal | Click "Assign To" button (FR-TASK-002) | SCR-005 (Task Detail) | P0 |
| Notification Toast | Toast | Task assigned (FR-NOTIF-001) | Global, persists across screens for short duration | P0 |

---

## 7. Content & Tone

### Voice & Tone
- **Overall Tone**: Professional, Direct, Empowering, and Helpful. Focus on clarity and efficiency.
- **Error Messages**: Concise, non-blaming, and actionable. Suggest next steps or alternative actions.
- **Empty States**: Encouraging, guiding the user on how to get started (e.g., "You have no tasks yet. Click 'Create Task' to add your first one!"). Always include a clear Call to Action (CTA).
- **Success Messages**: Brief, positive confirmation of action completion.
- **Confirmation Messages**: Clear, explicit, stating the action and requiring explicit user confirmation for irreversible actions.

### Content Guidelines
- **Headings**: Sentence case.
- **CTAs**: Action-oriented verbs (e.g., "Create Task", "Save Changes", "Assign Task", "Delete Task"). Avoid generic "Submit" or "Click Here".
- **Labels**: Concise, descriptive, always associated with their respective input fields.
- **Placeholder Text**: Provide helpful examples or format guidance (e.g., "Enter task title", "your.email@example.com"). Avoid "Lorem ipsum".

---

## 8. Data & Edge Cases

### Data Scenarios
| Scenario | Description | Handling |
|----------|-------------|----------|
| No Data | User has no tasks assigned or created. | SCR-003/Empty state: Display an encouraging message with a prominent "Create Task" CTA. |
| First Use | New user, no existing tasks or activity. | Similar to 'No Data', the SCR-003/Empty state guides the user to create their first task. |
| Large Data | 100+ tasks visible on dashboard. | SCR-003: Implement pagination (e.g., 20 items per page) or infinite scroll (if tested for performance) to manage display. |
| Slow Connection | Network latency >3s. | ALL: Display Skeleton screens (e.g., for SCR-003) or inline spinners (for form submissions) to indicate loading, preserving layout. |
| Offline | User loses network connectivity. | Display a global "Offline" banner/toast. For critical actions, provide feedback that action cannot be completed without connection. (MVP: Focus on online experience, gracefully degrade). |

### Edge Cases
| Case | Screen(s) Affected | Solution |
|------|-------------------|----------|
| Long text | SCR-003 (Task title/description), SCR-005 (Task title/description) | Task titles on dashboard (SCR-003) will truncate with an ellipsis and display full text on hover (tooltip). Descriptions within forms/details (SCR-004, SCR-005) will auto-wrap. |
| Missing image | N/A for MVP (no user-uploaded images). | For future use, implement a fallback placeholder icon/image. |
| Form validation | SCR-001, SCR-002, SCR-004, SCR-005 | Inline error messages below fields, red border highlighting the invalid field. Display a summary error at the top for multiple issues. |
| Session timeout | ALL authenticated screens | Upon API call returning authentication error, redirect user to SCR-002 (Login) with a message "Your session has expired. Please log in again." |
| Permissions | SCR-003, SCR-005, FR-TASK-003, FR-TASK-005 | Display read-only view or disable edit/delete actions for tasks the user does not have permission for. |
| Multiple Assignees | FR-TASK-002 | On SCR-003, display primary assignee and indicate "+X others". On SCR-005, list all assignees. |

---

## 9. Branding & Visual Direction

*See `designsystem.md` for all design tokens (colors, typography, spacing, shadows, etc.)*

### Branding Assets
- **Logo**: A simple, modern wordmark or icon+wordmark. Placeholder: `TaskFlow_Logo.svg`.
- **Icon Style**: Outlined icons for general UI actions. Filled icons for active states or critical actions. Consistent stroke weight and corner radius.
- **Illustration Style**: Minimalist, abstract, or geometric for empty states to convey clarity and simplicity, without being overly playful.
- **Photography Style**: Not applicable for MVP.

---

## 10. Component Specifications

*Component specifications defined in designsystem.md. Requirements per screen listed below.*

### Component Library Reference
**Source**: `.propel/context/docs/designsystem.md` (Component Specifications section)

### Required Components per Screen
| Screen ID | Components Required | Notes |
|-----------|---------------------|-------|
| SCR-001 | TextField (3), PasswordField (2), Button (1), Link (1), Alert (1) | Name, Email, Password, Confirm Password. Register Button, Login Link. Error/Success Alert. |
| SCR-002 | TextField (1), PasswordField (1), Button (1), Link (1), Alert (1) | Email, Password. Login Button, Forgot Password (future), Register Link. Error/Success Alert. |
| SCR-003 | Header (1), Button (1), Table (1), Dropdown (1), TextField (1, for search) | Header with Logo, Create Task Button, User Menu. Task Table, Status Filter Dropdown, Optional Search Field. Notification Toast/Badge. |
| SCR-004 | TextField (1), TextArea (1), Select (2), Button (2), Alert (1) | Title, Description. Status Dropdown, Priority Dropdown. Save Task Button, Cancel Button. Error/Success Alert. |
| SCR-005 | Header (1), TextField (1), TextArea (1), Select (2), Button (3), Link (1), Confirmation Modal (1), Assign Task Modal (1), Alert (1) | Header. Editable Title, Description. Status Dropdown, Priority Dropdown. Save, Cancel, Delete Buttons. Assign To Link. Delete Confirmation Modal, Assign Task Modal. |

### Component Summary
| Category | Components | Variants |
|----------|------------|----------|
| Actions | Button | Primary, Secondary, Tertiary, Ghost x S/M/L x Default/Hover/Focus/Active/Disabled/Loading |
| Inputs | TextField, PasswordField, TextArea, Select | Default/Focus/Error/Disabled x S/M/L |
| Navigation | Header | Default layout for web/tablet |
| Content | Table, Card (for individual task display if not tabular) | Standard for tasks, flexible content |
| Feedback | Modal, Toast, Alert, Skeleton Loader | Confirmation, Notification, Error/Success/Info, Content loading |
| Layout | Container, Grid, Stack | For structuring content and responsive layouts |

### Component Constraints
- Use only components from designsystem.md.
- No custom components without explicit approval after design review.
- All components must support all defined states (Default, Hover, Focus, Active, Disabled, Loading).
- Follow naming convention: `C/<Category>/<Name>` in Figma.

---

## 11. Prototype Flows

*Flows derived from use cases in spec.md. Each flow notes which personas it covers.*

### Flow: FL-001 - User Registration
**Flow ID**: FL-001
**Derived From**: UC-001
**Personas Covered**: Team Member, Team Manager
**Description**: User registers for a new TaskFlow account.

#### Flow Sequence
```
1. Entry: SCR-001 / Default (Register Account)
   - Trigger: User navigates to /register
   |
   v
2. Step: SCR-001 / Default (with input)
   - Action: User fills Name, Email, Password, Confirm Password.
   - Trigger: User clicks "Register"
   |
   v
3. Decision Point (Validation):
   +-- Success (UC-001 Main Flow) -> SCR-002 / Default (Login Page, success message)
   +-- Email Exists (UC-001 AF-1.1) -> SCR-001 / Error (inline email error)
   +-- Invalid Input (UC-001 AF-1.2) -> SCR-001 / Validation (inline field errors)
   +-- System Error (UC-001 AF-1.3) -> SCR-001 / Error (global error message)
```

#### Required Interactions
- Inline form validation feedback.
- Loading spinner on "Register" button during submission.
- Toast/Alert for success/failure after redirection.

### Flow: FL-002 - User Login
**Flow ID**: FL-002
**Derived From**: FR-USER-002
**Personas Covered**: Team Member, Team Manager
**Description**: User logs into their TaskFlow account.

#### Flow Sequence
```
1. Entry: SCR-002 / Default (Log In)
   - Trigger: User navigates to /login or redirected from /register
   |
   v
2. Step: SCR-002 / Default (with input)
   - Action: User fills Email, Password.
   - Trigger: User clicks "Log In"
   |
   v
3. Decision Point (Authentication):
   +-- Success (FR-USER-002 Main Flow) -> SCR-003 / Default (Task Dashboard)
   +-- Incorrect Credentials (FR-USER-002 Alt Flow) -> SCR-002 / Error (global "Invalid credentials" error)
   +-- System Error -> SCR-002 / Error (global "An unexpected error occurred" error)
```

#### Required Interactions
- Loading spinner on "Log In" button during submission.
- Error message for invalid credentials.

### Flow: FL-003 - Create New Task
**Flow ID**: FL-003
**Derived From**: UC-002, FR-TASK-001
**Personas Covered**: Team Member, Team Manager
**Description**: User creates a new task with title, description, status, and priority.

#### Flow Sequence
```
1. Entry: SCR-003 / Default (Task Dashboard)
   - Trigger: User clicks "Create Task" button in header
   |
   v
2. Step: SCR-004 / Default (Create Task Form)
   - Action: User fills Title, Description, selects Status and Priority.
   - Trigger: User clicks "Save Task"
   |
   v
3. Decision Point (Validation):
   +-- Success (UC-002 Main Flow) -> SCR-003 / Default (Task Dashboard, new task visible, success toast)
   +-- Missing Title (UC-002 AF-2.1) -> SCR-004 / Validation (inline title error)
   +-- System Error (UC-002 AF-2.2) -> SCR-004 / Error (global error message)
```

#### Required Interactions
- Inline form validation.
- Loading spinner on "Save Task" button.
- Success toast on dashboard after redirection.

### Flow: FL-004 - Manage Existing Task
**Flow ID**: FL-004
**Derived From**: FR-TASK-002, FR-TASK-003, FR-TASK-004, FR-TASK-005
**Personas Covered**: Team Member, Team Manager
**Description**: User modifies, assigns, marks complete, or deletes an existing task.

#### Flow Sequence
```
1. Entry: SCR-003 / Default (Task Dashboard)
   - Trigger: User clicks on a task title or an "Edit" action from dashboard context menu.
   |
   v
2. Step: SCR-005 / Default (Task Detail / Edit)
   - Action: User modifies fields (FR-TASK-003), clicks "Assign To" (FR-TASK-002), or clicks "Delete" (FR-TASK-005).
   - Trigger: User clicks "Save Changes", "Assign Task" (in modal), "Mark Complete", or "Delete".
   |
   v
3. Decision Point (Action Type):
   +-- Save Changes (FR-TASK-003):
   |   +-- Success -> SCR-003 / Default (Dashboard, updated task, success toast)
   |   +-- Invalid Input -> SCR-005 / Validation (inline errors)
   |   +-- System Error -> SCR-005 / Error (global error)
   |
   +-- Assign Task (FR-TASK-002, via Assign Task Modal):
   |   +-- Success -> SCR-005 / Default (Task Detail, updated assignees, success toast, triggers FR-NOTIF-001)
   |   +-- No User Selected -> Assign Task Modal / Validation
   |   +-- System Error -> Assign Task Modal / Error
   |
   +-- Mark Complete (FR-TASK-004):
   |   +-- Success -> SCR-003 / Default (Dashboard, status updated, success toast)
   |   +-- System Error -> SCR-005 / Error (global error)
   |
   +-- Delete Task (FR-TASK-005):
       - Trigger: User clicks "Delete" on SCR-005
       |
       v
       Confirmation Modal / Default
       - Trigger: User clicks "Confirm Delete"
       |
       v
       +-- Success -> SCR-003 / Default (Dashboard, task removed, success toast)
       +-- Cancel -> SCR-005 / Default
       +-- System Error -> Confirmation Modal / Error (global error)
```

#### Required Interactions
- Loading states for all save/update/delete actions.
- Confirmation dialog for delete actions.
- Success/error toasts.
- Searchable user list in Assign Task Modal.
- Notification (toast/badge) when a task is assigned.

### Flow: FL-005 - View and Filter Tasks
**Flow ID**: FL-005
**Derived From**: FR-DASH-001, FR-DASH-002
**Personas Covered**: Team Member, Team Manager
**Description**: User views their task dashboard and applies filters to narrow down the task list.

#### Flow Sequence
```
1. Entry: SCR-003 / Default (Task Dashboard)
   - Trigger: User logs in or navigates to dashboard
   |
   v
2. Step: SCR-003 / Default (Task List Display)
   - Action: User interacts with status filter dropdown.
   - Trigger: User selects one or more statuses (e.g., 'Open', 'In Progress').
   |
   v
3. Step: SCR-003 / Default (Filtered Task List)
   - Action: Task list updates to show only filtered tasks.
   - Trigger: User deselects filters.
   |
   v
4. Exit: SCR-003 / Default (All relevant tasks displayed again).
```

#### Required Interactions
- Instantaneous update of task list upon filter selection/deselection.
- Visual indication of active filters.
- Loading state (skeleton) for task list if filtering causes significant delay (e.g., fetching new data).

---

## 12. Export Requirements

### JPG Export Settings
| Setting | Value |
|---------|-------|
| Format | JPG |
| Quality | High (85%) |
| Scale - Tablet | 1.5x (for 768px base) |
| Scale - Web | 2x (for 1440px base) |
| Color Profile | sRGB |

### Export Naming Convention
`<AppName>__<Platform>__<ScreenName>__<State>__v<Version>.jpg`

### Export Manifest
| Screen | State | Platform | Filename |
|--------|-------|----------|----------|
| Register Account | Default | Web | TaskFlow__Web__RegisterAccount__Default__v1.jpg |
| Register Account | Loading | Web | TaskFlow__Web__RegisterAccount__Loading__v1.jpg |
| Register Account | Error | Web | TaskFlow__Web__RegisterAccount__Error__v1.jpg |
| Register Account | Validation | Web | TaskFlow__Web__RegisterAccount__Validation__v1.jpg |
| Log In | Default | Web | TaskFlow__Web__Login__Default__v1.jpg |
| Log In | Loading | Web | TaskFlow__Web__Login__Loading__v1.jpg |
| Log In | Error | Web | TaskFlow__Web__Login__Error__v1.jpg |
| Log In | Validation | Web | TaskFlow__Web__Login__Validation__v1.jpg |
| Task Dashboard | Default | Web | TaskFlow__Web__TaskDashboard__Default__v1.jpg |
| Task Dashboard | Loading | Web | TaskFlow__Web__TaskDashboard__Loading__v1.jpg |
| Task Dashboard | Empty | Web | TaskFlow__Web__TaskDashboard__Empty__v1.jpg |
| Task Dashboard | Error | Web | TaskFlow__Web__TaskDashboard__Error__v1.jpg |
| Create Task Form | Default | Web | TaskFlow__Web__CreateTaskForm__Default__v1.jpg |
| Create Task Form | Loading | Web | TaskFlow__Web__CreateTaskForm__Loading__v1.jpg |
| Create Task Form | Error | Web | TaskFlow__Web__CreateTaskForm__Error__v1.jpg |
| Create Task Form | Validation | Web | TaskFlow__Web__CreateTaskForm__Validation__v1.jpg |
| Task Detail / Edit | Default | Web | TaskFlow__Web__TaskDetailEdit__Default__v1.jpg |
| Task Detail / Edit | Loading | Web | TaskFlow__Web__TaskDetailEdit__Loading__v1.jpg |
| Task Detail / Edit | Error | Web | TaskFlow__Web__TaskDetailEdit__Error__v1.jpg |
| Task Detail / Edit | Validation | Web | TaskFlow__Web__TaskDetailEdit__Validation__v1.jpg |
| Register Account | Default | Tablet | TaskFlow__Tablet__RegisterAccount__Default__v1.jpg |
| Log In | Default | Tablet | TaskFlow__Tablet__Login__Default__v1.jpg |
| Task Dashboard | Default | Tablet | TaskFlow__Tablet__TaskDashboard__Default__v1.jpg |
| Create Task Form | Default | Tablet | TaskFlow__Tablet__CreateTaskForm__Default__v1.jpg |
| Task Detail / Edit | Default | Tablet | TaskFlow__Tablet__TaskDetailEdit__Default__v1.jpg |
| Confirmation Modal | Default | Web | TaskFlow__Web__ConfirmationModal__Default__v1.jpg |
| Assign Task Modal | Default | Web | TaskFlow__Web__AssignTaskModal__Default__v1.jpg |
| Notification Toast | Default | Web | TaskFlow__Web__NotificationToast__Default__v1.jpg |

### Total Export Count
- **Screens**: 5 core screens + 3 modals/overlays = 8 unique views
- **States per screen**: 4-5 states per core screen (average 4.8), 1-2 states for modals/overlays.
- **Platforms**: 2 (Web, Tablet)
- **Total JPGs**: Approximately (5 * 4.8 + 3 * 1) * 2 = 57 JPGs (plus specific modal states as individual exports) = 25 for Web + 5 for Tablet + 3 modals * 1 state * 1 platform (as they are overlays on web base) = 33 exports for V1.

---

## 13. Figma File Structure

### Page Organization
```
TaskFlow Figma File
+-- 00_Cover
|   +-- Project info, version, stakeholders, last updated
+-- 01_Foundations
|   +-- Color tokens (Light + Dark modes)
|   +-- Typography scale
|   +-- Spacing scale
|   +-- Radius tokens
|   +-- Elevation/shadows
|   +-- Grid definitions (Desktop: 12-col, Tablet: 8-col)
+-- 02_Components
|   +-- C/Actions/Button (Primary, Secondary, Ghost, Tertiary variants x S/M/L x all states)
|   +-- C/Actions/Link
|   +-- C/Inputs/TextField (Default, Password, Email, TextArea variants x S/M/L x all states)
|   +-- C/Inputs/Select (Default, variants x S/M/L x all states)
|   +-- C/Navigation/Header (Web/Tablet variants)
|   +-- C/Content/Table (Task Table variant)
|   +-- C/Feedback/Modal (Confirmation, Assign Task variants x Default/Loading/Error)
|   +-- C/Feedback/Toast (Success, Error, Info variants)
|   +-- C/Feedback/Alert (Success, Error, Info variants)
|   +-- C/Feedback/SkeletonLoader (for Table, Form)
|   +-- C/Layout/Divider
+-- 03_Patterns
|   +-- Auth Form Pattern (Login/Register layouts)
|   +-- Task Item Pattern (for table rows or card items)
|   +-- Filter Bar Pattern
|   +-- Empty State Pattern
|   +-- Error State Pattern
|   +-- Loading State Pattern (Skeleton)
+-- 04_Screens
|   +-- RegisterAccount/Default
|   +-- RegisterAccount/Loading
|   +-- RegisterAccount/Error
|   +-- RegisterAccount/Validation
|   +-- Login/Default
|   +-- Login/Loading
|   +-- Login/Error
|   +-- Login/Validation
|   +-- TaskDashboard/Default
|   +-- TaskDashboard/Loading
|   +-- TaskDashboard/Empty
|   +-- TaskDashboard/Error
|   +-- CreateTaskForm/Default
|   +-- CreateTaskForm/Loading
|   +-- CreateTaskForm/Error
|   +-- CreateTaskForm/Validation
|   +-- TaskDetailEdit/Default
|   +-- TaskDetailEdit/Loading
|   +-- TaskDetailEdit/Error
|   +-- TaskDetailEdit/Validation
+-- 05_Prototype
|   +-- FL-001: User Registration
|   +-- FL-002: User Login
|   +-- FL-003: Create New Task
|   +-- FL-004: Manage Existing Task
|   +-- FL-005: View and Filter Tasks
+-- 06_Handoff
    +-- Token usage rules (how to apply design tokens)
    +-- Component usage guidelines (props, variants, when to use)
    +-- Responsive specs (breakpoint behavior, layout shifts)
    +-- Edge cases (truncation, missing data, permissions)
    +-- Accessibility notes (focus management, ARIA suggestions, contrast checks)
```

---

## 14. Quality Checklist

### Pre-Export Validation
- [x] All screens have required states (Default/Loading/Empty/Error/Validation, where applicable).
- [x] All components use design tokens (no hard-coded values).
- [x] Color contrast meets WCAG AA (>=4.5:1 text, >=3:1 UI).
- [x] Focus states defined for all interactive elements.
- [x] Touch targets >= 44x44px for all interactive elements (even on tablet for larger finger targets).
- [x] Prototype flows wired and functional for all core user journeys.
- [x] Naming conventions followed for frames, components, and layers.
- [x] Export manifest complete and accurate.

### Post-Generation
- [x] `designsystem.md` updated with Figma references.
- [x] Export manifest generated.
- [x] JPG files named correctly.
- [x] Handoff documentation complete with all required sections.
- [x] All UXR are accounted for in the design.

---
## 15. Responsive Design Breakpoints

**Target Platforms**: Desktop & Tablet (NFR-USABILITY-001)

### Breakpoint Definitions
| Name | Min Width | Max Width | Grid Columns | Layout Adaptations |
|------|-----------|-----------|--------------|--------------------|
| Tablet | 768px | 1023px | 8 columns | Navigation: Header remains, potentially condensed. Task Dashboard: Card view or condensed table view. Forms: Stacked inputs. |
| Desktop Small | 1024px | 1439px | 12 columns | Navigation: Full header. Task Dashboard: Table view with more columns. Forms: Two-column layouts possible. |
| Desktop Large | 1440px | - | 12 columns | Optimized for wider content, increased white space, full task table with all details. |

### Layout Adaptations
- **Header**: Consistent across all breakpoints. Logo and key actions (Create Task, User Menu). Content adjusts spacing.
- **Forms (Register, Login, Create Task, Task Detail)**:
    - **Tablet**: All form fields stack vertically. Buttons span full width.
    - **Desktop**: Forms can utilize a two-column grid for better horizontal space distribution where logical (e.g., Status and Priority side-by-side).
- **Task Dashboard (SCR-003)**:
    - **Tablet**: Task list defaults to a simplified card view or a table with fewer columns, prioritizing key info (title, status, assignee). Horizontal scrolling may be introduced for detail columns. Filtering controls may switch to a bottom sheet or modal.
    - **Desktop**: Full table view, displaying all task properties (title, description snippet, status, priority, assignee, created by). Filtering controls remain visible inline.
- **Modals/Overlays**:
    - **Tablet**: Modals take up a larger percentage of the screen width, potentially 90% width. Drawers slide in from 100% width.
    - **Desktop**: Modals are centered, with a fixed max-width. Drawers slide in with a fixed width.

---
## 16. Accessibility Requirements (WCAG Compliance)

This section details the WCAG 2.2 Level AA accessibility requirements that will be strictly adhered to in the Figma designs.

### General Accessibility Principles
- **Perceivable**: Information and user interface components must be presentable to users in ways they can perceive.
- **Operable**: User interface components and navigation must be operable.
- **Understandable**: Information and the operation of user interface must be understandable.
- **Robust**: Content must be robust enough that it can be interpreted by a wide variety of user agents, including assistive technologies.

### Detailed Requirements

1.  **Color Contrast (WCAG 1.4.3 & 1.4.11)**
    *   **Text Contrast**: All standard text (normal size) MUST have a contrast ratio of at least 4.5:1 against its background. Large text (18pt+ regular or 14pt+ bold) MUST have a contrast ratio of at least 3:1.
    *   **Non-Text Contrast**: All interactive UI components (buttons, input borders, icons, focus indicators) and meaningful graphics MUST have a contrast ratio of at least 3:1 against adjacent colors.
    *   **Information Not by Color Alone**: Color MUST NOT be the only means of conveying information, indicating an action, prompting a response, or distinguishing a visual element (e.g., form errors use color AND text/icon).

2.  **Keyboard Operability (WCAG 2.1.1, 2.1.2, 2.1.4)**
    *   All functionality MUST be operable through a keyboard interface without requiring specific timings for individual keystrokes.
    *   **Focus Order**: Keyboard navigation (Tab key) MUST follow a logical and predictable order that makes sense to the user (left-to-right, top-to-bottom reading order).
    *   **Visible Focus Indicator**: All interactive elements (buttons, links, form fields, filter options) MUST display a clear and high-contrast visual focus indicator when tabbed to. The focus indicator MUST meet 3:1 contrast ratio against the component's default state and be at least 2px thick.
    *   **No Keyboard Traps**: Focus MUST NOT be trapped within any sub-section of content. Users must be able to move focus away from any component using only a keyboard.

3.  **Form Accessibility (WCAG 1.3.1, 3.3.1, 3.3.2)**
    *   **Labels**: All input fields (text, password, text area, select) MUST have a programmatically associated `<label>` element. Placeholder text is not a substitute for a label.
    *   **Error Identification**: Form validation errors MUST be clearly identified to the user. This includes visual indication (e.g., red border, error icon) AND text-based error messages (inline and/or summarized at the top of the form).
    *   **Error Suggestion**: Where input errors are detected and suggestions for correction are known, these suggestions MUST be provided (e.g., "Email already exists. Try logging in.").
    *   **Required Fields**: Required fields MUST be visually indicated (e.g., an asterisk and `(required)` in the label) and programmatically identified using `aria-required="true"`.

4.  **Semantic Structure (WCAG 1.3.1)**
    *   Use appropriate semantic HTML5 elements where possible (e.g., `<header>`, `<nav>`, `<main>`, `<footer>`, `<button>`, `<input>`, `<table>`).
    *   For custom interactive components or regions, appropriate ARIA roles, states, and properties (`role`, `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-current`, etc.) will be specified.

5.  **Image Accessibility (WCAG 1.1.1)**
    *   **Alt Text**: All `img` elements that convey information MUST have descriptive `alt` text.
    *   **Decorative Images**: Images that are purely decorative and convey no information MUST have empty `alt=""` attributes or be hidden from assistive technologies (`aria-hidden="true"`).

6.  **Navigation and Orientation (WCAG 2.4.1, 2.4.4, 2.4.5, 2.4.6)**
    *   **Page Titles**: Each screen MUST have a unique and descriptive title (e.g., `<title>TaskFlow - Task Dashboard</title>`).
    *   **Link Purpose**: The purpose of each link MUST be clear from its text or context. Avoid generic link text like "click here."
    *   **Headings**: Use a logical heading structure (`<h1>` to `<h6>`) to convey content hierarchy.
    *   **Landmark Regions**: Use HTML5 landmark elements (`<main>`, `<nav>`, `<aside>`, `<header>`, `<footer>`) or ARIA roles to define regions of the page.

7.  **Time-based Media (WCAG 2.2.1, 2.2.2)**
    *   Any content that moves, blinks, scrolls, or automatically updates for more than five seconds MUST have a mechanism for users to pause, stop, or hide it. (Applies to notifications, carousels, etc.).
    *   Auto-updating content (e.g., new task notifications) MUST have a mechanism to pause or adjust the timing of the refresh.

8.  **Responsive Design (WCAG 1.4.10)**
    *   Content MUST reflow gracefully at various screen sizes without loss of information or functionality, and without requiring two-dimensional scrolling. Text MUST be resizable up to 200% without loss of content or functionality.

9.  **User Preferences (WCAG 2.3.3)**
    *   The design MUST honor user preferences such as `prefers-reduced-motion` to minimize animations and transitions for users sensitive to motion.

### Figma Annotations
For handoff, specific annotations will be included in Figma:
- **Focus Order**: Numbered annotations indicating keyboard tab sequence.
- **ARIA Suggestions**: Notes on specific `aria-label`, `role`, `aria-current`, etc., for custom components.
- **Heading Levels**: Text annotations for `<h1>`, `<h2>`, etc., for screen titles and major sections.
- **Error States**: Detailed descriptions of error message association and visual treatment.
- **Contrast Ratios**: Notes on critical text and UI elements confirming contrast ratios.
- **Touch Targets**: Callouts for mobile/tablet interactive elements confirming 44x44px minimum.
- **Motion Spec**: Detailed timing and easing for interactive feedback and transitions.

By systematically addressing these requirements during the design process, TaskFlow will achieve WCAG 2.2 Level AA compliance, providing an inclusive and usable experience for all users.