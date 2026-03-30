# Figma Design Specification - TaskFlow

## 1. Figma Specification
**Platform**: Web / Responsive

---

## 2. Source References

### Primary Source
| Document | Path | Purpose |
|----------|------|---------|
| Requirements | `.propel/context/docs/spec.md` | Personas, use cases, epics with UI impact flags |

### Optional Sources
| Document | Path | Purpose |
|----------|------|---------|
| Wireframes | `.propel/context/wireframes/` | Entity understanding, content structure (Not provided, implicit) |
| Design Assets | `.propel/context/Design/` | Visual references from spec.md epics (To be generated) |

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
| UXR-101 | Usability | System MUST provide a clear and intuitive user registration process. | 1. User can complete registration with required fields (Name, Email, Password).<br/>2. Password strength requirements are clearly communicated before submission. | SCR-001 |
| UXR-102 | Usability | System MUST allow registered users to log in efficiently. | 1. User can successfully log in using correct credentials and be redirected to the dashboard.<br/>2. Login form fields are clearly labeled and accessible. | SCR-002 |
| UXR-103 | Usability | System MUST enable users to create tasks easily and quickly. | 1. Task creation form is easily discoverable and navigable.<br/>2. Form fields for Title, Description, Status, and Priority are clearly presented with sensible defaults. | SCR-004 |
| UXR-104 | Usability | System MUST provide a clear mechanism for assigning tasks to team members. | 1. An "Assign To" option is readily available in task detail/edit views.<br/>2. The assignee selection interface provides a searchable list of users for efficient selection. | SCR-005 |
| UXR-105 | Usability | System MUST allow users to modify task details intuitively. | 1. Task details (Title, Description, Status, Priority) are pre-filled in an editable form.<br/>2. "Save Changes" action is clearly identifiable and provides feedback. | SCR-005 |
| UXR-106 | Usability | System MUST provide a quick action to mark a task as 'Completed'. | 1. A dedicated UI element (e.g., checkbox, button) allows users to change task status to 'Completed' with a single interaction.<br/>2. The action is discoverable and provides immediate visual feedback. | SCR-003, SCR-005 |
| UXR-107 | Usability | System MUST prevent accidental task deletion. | 1. A confirmation prompt is displayed before a task is permanently deleted.<br/>2. Confirmation message clearly states the irreversible nature of the action. | SCR-003, SCR-005 |
| UXR-108 | Usability | System MUST display a clear and comprehensive task dashboard. | 1. The dashboard visually distinguishes between tasks created by the user, assigned to the user, and team tasks.<br/>2. Each task entry clearly shows its Title, Status, Priority, and Assignee(s). | SCR-003 |
| UXR-109 | Usability | System MUST ensure dashboard data reflects the latest changes promptly. | 1. Task creation, updates, and deletions are reflected on the dashboard within 200ms.<br/>2. Loading states are provided for data fetching. | SCR-003 |
| UXR-110 | Usability | System MUST provide intuitive filtering options for tasks on the dashboard. | 1. Filter controls for task status are clearly visible and easy to interact with.<br/>2. Applying or clearing filters immediately updates the displayed task list. | SCR-003 |
| UXR-200 | Accessibility | All UI elements MUST meet WCAG 2.2 Level AA compliance. | 1. All interactive elements have appropriate ARIA attributes for screen readers.<br/>2. Keyboard navigation is fully supported for all flows. | All Screens |
| UXR-201 | Accessibility | Registration form MUST be accessible to users of assistive technologies. | 1. All input fields are programmatically associated with visible labels.<br/>2. Error messages are linked to their respective input fields via `aria-describedby`. | SCR-001 |
| UXR-202 | Accessibility | Login form MUST be accessible to users of assistive technologies. | 1. All input fields are programmatically associated with visible labels.<br/>2. Error messages are clearly communicated to screen readers. | SCR-002 |
| UXR-203 | Accessibility | Task creation form MUST be accessible to users of assistive technologies. | 1. Input fields and dropdowns are programmatically associated with visible labels.<br/>2. Validation errors are communicated effectively to screen readers. | SCR-004 |
| UXR-204 | Accessibility | Task assignment user search/select interface MUST be accessible. | 1. Search input has an appropriate label.<br/>2. List of users is navigable and selectable via keyboard and screen reader.<br/>3. Selection status is clearly announced. | SCR-005 (Assign Task Modal) |
| UXR-205 | Accessibility | Dashboard task list/table MUST be accessible. | 1. If tabular, column headers are properly associated with data cells (e.g., `<th>` with `scope`).<br/>2. Keyboard users can navigate and interact with tasks. | SCR-003 |
| UXR-206 | Accessibility | Dashboard filter controls MUST be accessible. | 1. Filter options are clearly labeled and their state (selected/unselected) is programmatically discernible.<br/>2. Users can interact with filters using only a keyboard. | SCR-003 |
| UXR-207 | Accessibility | All text and UI elements MUST meet minimum color contrast requirements. | 1. Normal text contrast ratio is >= 4.5:1.<br/>2. Large text (18pt+/14pt+ bold) contrast ratio is >= 3:1.<br/>3. Non-text UI components (icons, borders) contrast ratio is >= 3:1. | All Screens |
| UXR-208 | Accessibility | All interactive elements MUST have clearly visible focus states. | 1. Focus indicator (e.g., outline) is visible and has a contrast ratio of >= 3:1 against adjacent colors.<br/>2. Focus indicator is at least 2px thick and has an offset. | All Screens |
| UXR-209 | Accessibility | Mobile touch targets MUST be adequately sized. | 1. Interactive elements on mobile breakpoints have a minimum target size of 44x44px. | All Screens (Mobile Breakpoint) |
| UXR-210 | Accessibility | All interactive elements MUST use appropriate semantic HTML or ARIA roles and properties. | 1. Buttons use `<button>`, links use `<a>`, forms use `<form>`, etc.<br/>2. Custom interactive elements have appropriate ARIA roles (e.g., `role="dialog"`, `aria-label`). | All Screens |
| UXR-211 | Accessibility | All interactive elements MUST be keyboard operable and follow a logical tab order. | 1. Users can navigate sequentially through all interactive elements using Tab/Shift+Tab.<br/>2. Focus does not get trapped within any component (e.g., modal can be dismissed with Escape). | All Screens |
| UXR-212 | Accessibility | Dynamic content updates MUST be announced to screen readers. | 1. Use of ARIA live regions for notifications (e.g., task assignment confirmation, error messages) when they appear dynamically. | SCR-003, SCR-005 |
| UXR-301 | Responsiveness | UI MUST adapt gracefully across desktop and tablet screen sizes (768px and above). | 1. Layouts automatically adjust for varying screen widths without horizontal scrolling.<br/>2. Content reflows and scales appropriately to prevent truncation or overlap. | All Screens |
| UXR-302 | Responsiveness | Key interactive elements MUST be appropriately sized and positioned for touch and mouse interaction on all supported devices. | 1. Buttons, form fields, and navigation items maintain comfortable touch/click targets on tablets and desktops.<br/>2. Spacing between elements ensures usability on touch devices. | All Screens |
| UXR-401 | Visual Design | All UI components and layouts MUST adhere strictly to the defined design system tokens. | 1. Colors, typography, spacing, and border radii used are exclusively from `designsystem.md`.<br/>2. Consistency in visual styling is maintained across all screens and components. | All Screens |
| UXR-501 | Interaction | System MUST provide clear feedback upon task assignment. | 1. Assigned users receive an in-app toast notification or visual badge update.<br/>2. The notification clearly indicates the assigned task and assigner. | SCR-003, SCR-005 |
| UXR-502 | Interaction | System MUST provide immediate visual feedback for task status updates. | 1. When a task status is changed (e.g., to 'Completed'), the UI reflects this change without a page reload.<br/>2. A subtle animation or color change highlights the update. | SCR-003, SCR-005 |
| UXR-503 | Interaction | System MUST provide clear visual confirmation and feedback for task deletion. | 1. After confirmation, the task visually disappears from the list/dashboard.<br/>2. A brief success message (e.g., toast) confirms deletion. | SCR-003, SCR-005 |
| UXR-504 | Interaction | System MUST provide immediate visual feedback when filters are applied/cleared on the dashboard. | 1. The task list updates without noticeable delay (within 200ms) after filter changes.<br/>2. A loading indicator appears if the update takes longer than 300ms. | SCR-003 |
| UXR-505 | Interaction | In-app notifications MUST be clear, concise, and non-intrusive. | 1. Notifications (e.g., toast messages for success/error) are brief and appear for a limited duration.<br/>2. They provide sufficient information without blocking user workflow. | SCR-003, SCR-005 |
| UXR-601 | Error Handling | System MUST display specific and actionable inline error messages for registration failures. | 1. When validation fails (e.g., email taken, invalid password), specific error messages appear next to the problematic fields.<br/>2. Global errors (e.g., system error) are handled with a generic message. | SCR-001 |
| UXR-602 | Error Handling | System MUST display a generic, non-specific error message for invalid login credentials. | 1. For incorrect email or password, a single message "Invalid credentials" is shown, without distinguishing between the two. | SCR-002 |
| UXR-603 | Error Handling | System MUST provide inline error messages for mandatory task creation fields. | 1. An error message "Task Title is required" appears next to the title field if it's left empty.<br/>2. System errors are handled with a generic message and retry option. | SCR-004 |
| UXR-604 | Error Handling | System MUST provide inline error messages for mandatory task edit fields. | 1. An error message appears if a mandatory field (e.g., Title) is left empty during task modification. | SCR-005 |

### UXR Categories
- **Usability**: Navigation, discoverability, efficiency (max 3 clicks, clear hierarchy)
- **Accessibility**: WCAG 2.2 AA compliance, assistive technology support
- **Responsiveness**: Breakpoint behavior, viewport adaptation
- **Visual Design**: Design system adherence, consistency
- **Interaction**: Feedback, loading states, animations (response within 200ms)
- **Error Handling**: Error messages, recovery paths

### UXR Derivation Logic
- **Usability UXR**: Derived from UC-XXX success paths (navigation depth, discoverability)
- **Accessibility UXR**: Derived from WCAG 2.2 AA standards + designsystem.md constraints
- **Responsiveness UXR**: Derived from platform targets + breakpoint definitions
- **Visual Design UXR**: Derived from designsystem.md token requirements
- **Interaction UXR**: Derived from flow complexity + state transitions
- **Error Handling UXR**: Derived from UC-XXX alternative/exception paths

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

| Persona         | Role          | Primary Goals                                                                         | Key Screens                   |
|-----------------|---------------|---------------------------------------------------------------------------------------|-------------------------------|
| Team Member     | End User      | Execute tasks, collaborate, update progress, clear assignments.                       | Dashboard, Create Task, Task Detail |
| Team Manager    | End User      | Assign tasks, track progress, ensure accountability, identify blockers.                 | Dashboard, Create Task, Task Detail |

---

## 5. Information Architecture

### Site Map
```
TaskFlow
+-- Authentication
|   +-- Register Account (SCR-001)
|   +-- Login (SCR-002)
+-- Dashboard (SCR-003)
|   +-- Create Task (SCR-004)
|   +-- Task Detail / Edit (SCR-005)
```

### Navigation Patterns
| Pattern       | Type         | Platform Behavior                                    |
|---------------|--------------|------------------------------------------------------|
| Primary Nav   | Header/Menu  | Desktop: Persistent Header with "New Task" button and User Menu. Mobile: Hamburger menu or prominent FAB for "New Task". |
| Secondary Nav | Filters      | Dashboard: Inline filter controls for task status.   |
| Utility Nav   | User Menu    | Access to Logout.                                    |

---

## 6. Screen Inventory

*All screens derived from use cases in spec.md*

### Screen List
| Screen ID | Screen Name           | Derived From             | Personas Covered        | Priority | States Required                       |
|-----------|-----------------------|--------------------------|-------------------------|----------|---------------------------------------|
| SCR-001   | Register Account      | UC-001, FR-USER-001      | Team Member, Team Manager | P0       | Default, Loading, Error, Validation     |
| SCR-002   | Login                 | FR-USER-002              | Team Member, Team Manager | P0       | Default, Loading, Error, Validation     |
| SCR-003   | Dashboard             | FR-DASH-001, FR-DASH-002, UC-002, UC-003, UC-004, UC-005 | All                     | P0       | Default, Loading, Empty, Error        |
| SCR-004   | Create Task           | UC-002, FR-TASK-001      | Team Member, Team Manager | P0       | Default, Loading, Error, Validation     |
| SCR-005   | Task Detail / Edit    | UC-003, FR-TASK-002, FR-TASK-003, FR-TASK-004, FR-TASK-005 | All                     | P0       | Default, Loading, Error, Validation     |

### Priority Legend
- **P0**: Critical path (must-have for MVP)
- **P1**: Core functionality (high priority)
- **P2**: Important features (medium priority)
- **P3**: Nice-to-have (low priority)

### Screen-to-Persona Coverage Matrix
| Screen            | Team Member | Team Manager | Notes                               |
|-------------------|-------------|--------------|-------------------------------------|
| Register Account  | Primary     | Primary      | Entry point for new users.          |
| Login             | Primary     | Primary      | All users need to log in.           |
| Dashboard         | Primary     | Primary      | Core view for all task management.  |
| Create Task       | Primary     | Primary      | Both roles can create tasks.        |
| Task Detail / Edit | Primary     | Primary      | Both roles interact with task details. |

### Modal/Overlay Inventory
| Name                 | Type    | Trigger                     | Parent Screen(s)              | Priority |
|----------------------|---------|-----------------------------|-------------------------------|----------|
| Confirm Delete Task  | Modal   | Click "Delete Task"         | SCR-003, SCR-005              | P0       |
| Assign Task Selector | Modal/Drawer | Click "Assign To"           | SCR-005                       | P0       |
| Global Toast         | Toast   | Success/Error messages (e.g., "Task created successfully") | All authenticated screens     | P0       |

---

## 7. Content & Tone

### Voice & Tone
- **Overall Tone**: Professional, clear, concise, and encouraging. Aims for a friendly but efficient user experience.
- **Error Messages**: Helpful, non-blaming, actionable, and specific where possible. Avoid jargon.
- **Empty States**: Encouraging, guiding the user on how to get started, with clear calls-to-action (CTAs).
- **Success Messages**: Brief, positive, and acknowledge the completion of an action, often suggesting next steps.

### Content Guidelines
- **Headings**: Sentence case.
- **CTAs**: Action-oriented, specific verbs (e.g., "Create Task", "Save Changes", "Delete Task", "Assign").
- **Labels**: Concise, descriptive, and positioned clearly with their associated input fields.
- **Placeholder Text**: Provide helpful examples or format guidance, not "Lorem ipsum" in final designs.

---

## 8. Data & Edge Cases

### Data Scenarios
| Scenario          | Description                             | Handling                                         |
|-------------------|-----------------------------------------|--------------------------------------------------|
| No Data (Dashboard)| User has no tasks created or assigned   | SCR-003/Empty state: Illustration, message, "Create New Task" CTA. |
| First Use         | New user logs in for the first time     | SCR-003/Empty state serves as initial onboarding.|
| Large Data (Tasks)| User has many tasks (e.g., 50+)         | Pagination or infinite scroll for task lists (design for up to 100+ items). |
| Slow Connection   | Network latency >3 seconds              | Skeleton screens for SCR-003, SCR-005. Inline spinners for form submissions. |
| Offline           | No network connectivity                 | Graceful degradation (e.g., show cached data if applicable, "No Internet Connection" message). |

### Edge Cases
| Case                | Screen(s) Affected           | Solution                                         |
|---------------------|------------------------------|--------------------------------------------------|
| Long Task Title     | SCR-003, SCR-004, SCR-005    | Truncation with ellipsis (`...`) and tooltip on hover for full text. |
| Long Description    | SCR-005                      | Expandable text area or scrollable content.      |
| Missing Assignee    | SCR-003, SCR-005             | "Unassigned" placeholder clearly displayed.      |
| Form Validation     | SCR-001, SCR-002, SCR-004, SCR-005 | Inline error messages, red border highlighting invalid fields. |
| Permission Denied   | SCR-005 (Edit/Delete)        | Disable relevant buttons/actions, or show "Access Denied" message if direct URL accessed without permission. |

---

## 9. Branding & Visual Direction

*See `designsystem.md` for all design tokens (colors, typography, spacing, shadows, etc.)*

### Branding Assets
- **Logo**: "TaskFlow" wordmark, clean sans-serif typography, potentially with a minimalist icon representing flow/tasks.
- **Icon Style**: Outlined, simple, universally recognizable icons.
- **Illustration Style**: Minimalist, geometric, functional, used primarily for empty states.
- **Photography Style**: Not applicable for MVP.

---

## 10. Component Specifications

*Component specifications defined in designsystem.md. Requirements per screen listed below.*

### Component Library Reference
**Source**: `.propel/context/docs/designsystem.md` (Component Specifications section)

### Required Components per Screen
| Screen ID | Components Required                                | Notes                                            |
|-----------|----------------------------------------------------|--------------------------------------------------|
| SCR-001   | TextField (3), Button (1), Link (1)              | Name, Email, Password, Register, Login link      |
| SCR-002   | TextField (2), Button (1), Link (1)              | Email, Password, Login, Register link            |
| SCR-003   | Header (1), TaskCard (N) or Table (1), Button (1), Select (1) | App Header, list of tasks, "New Task" CTA, Status Filter |
| SCR-004   | TextField (2), TextArea (1), Select (2), Button (2) | Task Title, Description, Status, Priority, Save, Cancel |
| SCR-005   | Header (1), TextField (1), TextArea (1), Select (2), Button (4) | Task Title, Description, Status, Priority, Assign, Save, Delete, Complete |

### Component Summary
| Category    | Components                               | Variants                                       |
|-------------|------------------------------------------|------------------------------------------------|
| Actions     | Button, IconButton, Link                 | Primary, Secondary, Ghost, Danger x S/M/L x States |
| Inputs      | TextField, TextArea, Select, Checkbox, Radio | States (Default, Focus, Error, Disabled)        |
| Navigation  | Header                                   | App Header (Desktop), simplified (Mobile)       |
| Content     | Card, Table, Avatar, Badge               | TaskCard, TaskTable (for Dashboard), StatusBadge |
| Feedback    | Modal, Toast, Skeleton                   | Confirmation Modal, Success/Error Toast, Skeleton Loader |

### Component Constraints
- Use only components from designsystem.md.
- No custom components without explicit approval and documentation in designsystem.md.
- All components must support all defined states (Default, Hover, Focus, Active, Disabled, Loading).
- Follow naming convention: `C/<Category>/<Name>`.

---

## 11. Prototype Flows

*Flows derived from use cases in spec.md. Each flow notes which personas it covers.*

### Flow: FL-001 - Register Account (Success)
**Flow ID**: FL-001
**Derived From**: UC-001 (Main Flow)
**Personas Covered**: Team Member, Team Manager
**Description**: User successfully registers a new account and is redirected to the login page.

#### Flow Sequence
```
1. Entry: Register Account (SCR-001) / Default
   - Trigger: User lands on registration page.
   |
   v
2. Step: Register Account (SCR-001) / Default (Form Filled)
   - Action: User enters Name, Email, Password, confirms Password. Clicks "Register".
   |
   v
3. Step: System Processing / Loading
   - Action: System validates and creates account (brief loading state).
   |
   v
4. Exit: Login (SCR-002) / Default
   - Action: Success message (e.g., Toast) followed by redirect to login.
```

#### Required Interactions
- Form submission, showing loading state.
- Success Toast message.
- Page redirection.

### Flow: FL-002 - Register Account (Email Already Exists)
**Flow ID**: FL-002
**Derived From**: UC-001 (AF-1.1)
**Personas Covered**: Team Member, Team Manager
**Description**: User attempts to register with an existing email, resulting in an inline error.

#### Flow Sequence
```
1. Entry: Register Account (SCR-001) / Default
   - Trigger: User lands on registration page.
   |
   v
2. Step: Register Account (SCR-001) / Default (Form Filled)
   - Action: User enters Name, Email (already exists), Password, confirms Password. Clicks "Register".
   |
   v
3. Step: Register Account (SCR-001) / Error (Validation)
   - Action: Inline error message "This email address is already in use." appears next to Email field.
```

#### Required Interactions
- Form submission.
- Inline error display (color, message).

### Flow: FL-003 - Login (Success)
**Flow ID**: FL-003
**Derived From**: FR-USER-002 (Main Flow)
**Personas Covered**: Team Member, Team Manager
**Description**: User successfully logs into the system and is redirected to the Dashboard.

#### Flow Sequence
```
1. Entry: Login (SCR-002) / Default
   - Trigger: User lands on login page.
   |
   v
2. Step: Login (SCR-002) / Default (Form Filled)
   - Action: User enters registered Email and Password. Clicks "Login".
   |
   v
3. Step: System Processing / Loading
   - Action: System authenticates user (brief loading state).
   |
   v
4. Exit: Dashboard (SCR-003) / Default
   - Action: Redirect to the main task dashboard.
```

#### Required Interactions
- Form submission, showing loading state.
- Page redirection.

### Flow: FL-004 - Login (Invalid Credentials)
**Flow ID**: FL-004
**Derived From**: FR-USER-002 (AF-Incorrect Credentials)
**Personas Covered**: Team Member, Team Manager
**Description**: User attempts to log in with incorrect credentials, resulting in a generic error.

#### Flow Sequence
```
1. Entry: Login (SCR-002) / Default
   - Trigger: User lands on login page.
   |
   v
2. Step: Login (SCR-002) / Default (Form Filled)
   - Action: User enters incorrect Email or Password. Clicks "Login".
   |
   v
3. Step: Login (SCR-002) / Error
   - Action: Global error message "Invalid credentials" appears (e.g., at top of form or via Toast).
```

#### Required Interactions
- Form submission.
- Error message display.

### Flow: FL-005 - Create Task (Success)
**Flow ID**: FL-005
**Derived From**: UC-002, FR-TASK-001 (Main Flow)
**Personas Covered**: Team Member, Team Manager
**Description**: User successfully creates a new task and sees it on the Dashboard.

#### Flow Sequence
```
1. Entry: Dashboard (SCR-003) / Default
   - Trigger: User clicks "New Task" button.
   |
   v
2. Step: Create Task (SCR-004) / Default
   - Action: User fills Task Title, Description, selects Status and Priority. Clicks "Save Task".
   |
   v
3. Step: System Processing / Loading
   - Action: System creates task (brief loading state).
   |
   v
4. Exit: Dashboard (SCR-003) / Default
   - Action: Success message (Toast) and redirect back to Dashboard, showing the new task.
```

#### Required Interactions
- Button click, form input, dropdown selection.
- Loading state for form submission.
- Success Toast.
- Dashboard update.

### Flow: FL-006 - Create Task (Missing Title)
**Flow ID**: FL-006
**Derived From**: UC-002 (AF-2.1), FR-TASK-001 (AF-Missing Title)
**Personas Covered**: Team Member, Team Manager
**Description**: User attempts to create a task without a title, resulting in an inline error.

#### Flow Sequence
```
1. Entry: Create Task (SCR-004) / Default
   - Trigger: User lands on create task form.
   |
   v
2. Step: Create Task (SCR-004) / Default (Form Partially Filled)
   - Action: User leaves Task Title empty, fills other fields. Clicks "Save Task".
   |
   v
3. Step: Create Task (SCR-004) / Validation
   - Action: Inline error message "Task Title is required." appears next to the Title field.
```

#### Required Interactions
- Form submission.
- Inline error display.

### Flow: FL-007 - View and Filter Dashboard
**Flow ID**: FL-007
**Derived From**: FR-DASH-001, FR-DASH-002
**Personas Covered**: Team Member, Team Manager
**Description**: User views the dashboard and applies a filter.

#### Flow Sequence
```
1. Entry: Dashboard (SCR-003) / Default
   - Trigger: User logs in or navigates to Dashboard.
   |
   v
2. Step: Dashboard (SCR-003) / Default (Viewing All Tasks)
   - Action: User observes task list.
   |
   v
3. Step: Dashboard (SCR-003) / Default (Filter Applied)
   - Action: User selects "Status: Completed" from the filter options.
   |
   v
4. Exit: Dashboard (SCR-003) / Default (Filtered View)
   - Action: Dashboard immediately updates to show only tasks with 'Completed' status.
```

#### Required Interactions
- Filter selection (e.g., checkbox, dropdown).
- Immediate update of task list (visual feedback).

### Flow: FL-008 - Edit Task (Change Status to Completed)
**Flow ID**: FL-008
**Derived From**: FR-TASK-004
**Personas Covered**: Team Member, Team Manager
**Description**: User quickly marks an existing task as completed from the dashboard.

#### Flow Sequence
```
1. Entry: Dashboard (SCR-003) / Default
   - Trigger: User views a task on the dashboard.
   |
   v
2. Step: Dashboard (SCR-003) / Default (Task Selected)
   - Action: User clicks the "Mark as Completed" button/checkbox associated with a specific task.
   |
   v
3. Step: System Processing / Loading
   - Action: System updates task status (brief inline loading for the specific task).
   |
   v
4. Exit: Dashboard (SCR-003) / Default
   - Action: The task's status visibly changes to 'Completed' on the dashboard.
```

#### Required Interactions
- Quick action (button/checkbox click).
- Inline loading/spinner for the specific task.
- Immediate visual update of task status.

### Flow: FL-009 - Assign Task
**Flow ID**: FL-009
**Derived From**: FR-TASK-002
**Personas Covered**: Team Member, Team Manager
**Description**: User assigns an existing task to another team member.

#### Flow Sequence
```
1. Entry: Task Detail / Edit (SCR-005) / Default
   - Trigger: User navigates to a task detail page.
   |
   v
2. Step: Task Detail / Edit (SCR-005) / Default
   - Action: User clicks "Assign To" button.
   |
   v
3. Step: Assign Task Selector (Modal/Drawer) / Default
   - Action: User searches for and selects one or more team members from the list. Clicks "Confirm Assignment".
   |
   v
4. Step: System Processing / Loading
   - Action: System updates assignment (brief loading state on modal).
   |
   v
5. Exit: Task Detail / Edit (SCR-005) / Default
   - Action: Success message (Toast), modal closes, assigned user(s) names appear on task detail.
```

#### Required Interactions
- Button click to open modal/drawer.
- Search input and selection from list.
- Confirmation button.
- Loading state for modal.
- Success Toast and UI update.

### Flow: FL-010 - Delete Task
**Flow ID**: FL-010
**Derived From**: FR-TASK-005
**Personas Covered**: Team Member, Team Manager
**Description**: User deletes an existing task with confirmation.

#### Flow Sequence
```
1. Entry: Task Detail / Edit (SCR-005) / Default
   - Trigger: User views a task detail page.
   |
   v
2. Step: Task Detail / Edit (SCR-005) / Default
   - Action: User clicks "Delete Task" button.
   |
   v
3. Step: Confirm Delete Task (Modal) / Default
   - Action: User sees confirmation message. Clicks "Confirm Delete".
   |
   v
4. Step: System Processing / Loading
   - Action: System deletes task (brief loading state on modal).
   |
   v
5. Exit: Dashboard (SCR-003) / Default
   - Action: Success message (Toast) and redirect to Dashboard; deleted task is no longer visible.
```

#### Required Interactions
- Button click to open modal.
- Confirmation button click.
- Loading state for modal.
- Success Toast.
- Dashboard update and removal of task.

---

## 12. Export Requirements

### JPG Export Settings
| Setting      | Value        |
|--------------|--------------|
| Format       | JPG          |
| Quality      | High (85%)   |
| Scale - Mobile | 2x           |
| Scale - Web    | 2x           |
| Color Profile| sRGB         |

### Export Naming Convention
`<AppName>__<Platform>__<ScreenName>__<State>__v<Version>.jpg`

### Export Manifest
| Screen                | State       | Platform | Filename                                 |
|-----------------------|-------------|----------|------------------------------------------|
| Register Account      | Default     | Web      | TaskFlow__Web__RegisterAccount__Default__v1.jpg |
| Register Account      | Loading     | Web      | TaskFlow__Web__RegisterAccount__Loading__v1.jpg |
| Register Account      | Error       | Web      | TaskFlow__Web__RegisterAccount__Error__v1.jpg |
| Register Account      | Validation  | Web      | TaskFlow__Web__RegisterAccount__Validation__v1.jpg |
| Login                 | Default     | Web      | TaskFlow__Web__Login__Default__v1.jpg    |
| Login                 | Loading     | Web      | TaskFlow__Web__Login__Loading__v1.jpg    |
| Login                 | Error       | Web      | TaskFlow__Web__Login__Error__v1.jpg      |
| Login                 | Validation  | Web      | TaskFlow__Web__Login__Validation__v1.jpg |
| Dashboard             | Default     | Web      | TaskFlow__Web__Dashboard__Default__v1.jpg |
| Dashboard             | Loading     | Web      | TaskFlow__Web__Dashboard__Loading__v1.jpg |
| Dashboard             | Empty       | Web      | TaskFlow__Web__Dashboard__Empty__v1.jpg  |
| Dashboard             | Error       | Web      | TaskFlow__Web__Dashboard__Error__v1.jpg  |
| Create Task           | Default     | Web      | TaskFlow__Web__CreateTask__Default__v1.jpg |
| Create Task           | Loading     | Web      | TaskFlow__Web__CreateTask__Loading__v1.jpg |
| Create Task           | Error       | Web      | TaskFlow__Web__CreateTask__Error__v1.jpg |
| Create Task           | Validation  | Web      | TaskFlow__Web__CreateTask__Validation__v1.jpg |
| Task Detail / Edit    | Default     | Web      | TaskFlow__Web__TaskDetailEdit__Default__v1.jpg |
| Task Detail / Edit    | Loading     | Web      | TaskFlow__Web__TaskDetailEdit__Loading__v1.jpg |
| Task Detail / Edit    | Error       | Web      | TaskFlow__Web__TaskDetailEdit__Error__v1.jpg |
| Task Detail / Edit    | Validation  | Web      | TaskFlow__Web__TaskDetailEdit__Validation__v1.jpg |
| Register Account      | Default     | Mobile   | TaskFlow__Mobile__RegisterAccount__Default__v1.jpg |
| Login                 | Default     | Mobile   | TaskFlow__Mobile__Login__Default__v1.jpg |
| Dashboard             | Default     | Mobile   | TaskFlow__Mobile__Dashboard__Default__v1.jpg |
| Create Task           | Default     | Mobile   | TaskFlow__Mobile__CreateTask__Default__v1.jpg |
| Task Detail / Edit    | Default     | Mobile   | TaskFlow__Mobile__TaskDetailEdit__Default__v1.jpg |


### Total Export Count
- **Screens**: 5
- **States per screen**: 4-5 (4 for auth, 5 for dashboard/forms)
- **Total JPGs**: Approximately 20 (Web) + 5 (Mobile) = 25

---

## 13. Figma File Structure

### Page Organization
```
TaskFlow Figma File
+-- 00_Cover
|   +-- Project info, version, stakeholders
+-- 01_Foundations
|   +-- Color tokens (Light + Dark)
|   +-- Typography scale
|   +-- Spacing scale
|   +-- Radius tokens
|   +-- Elevation/shadows
|   +-- Grid definitions (12-column responsive)
+-- 02_Components
|   +-- C/Actions/Button
|   +-- C/Actions/IconButton
|   +-- C/Actions/Link
|   +-- C/Inputs/TextField
|   +-- C/Inputs/TextArea
|   +-- C/Inputs/Select
|   +-- C/Inputs/Checkbox
|   +-- C/Inputs/Radio
|   +-- C/Navigation/Header
|   +-- C/Content/Card (TaskCard)
|   +-- C/Content/Table (TaskTable)
|   +-- C/Content/Badge (StatusBadge)
|   +-- C/Feedback/Modal
|   +-- C/Feedback/Toast
|   +-- C/Feedback/Skeleton
+-- 03_Patterns
|   +-- Auth form pattern (Login, Register)
|   +-- Task form pattern (Create, Edit)
|   +-- Task list pattern (Dashboard table/cards)
|   +-- Empty state pattern
|   +-- Loading pattern (Skeletons)
|   +-- Confirmation modal pattern
+-- 04_Screens
|   +-- RegisterAccount/Default
|   +-- RegisterAccount/Loading
|   +-- RegisterAccount/Error
|   +-- RegisterAccount/Validation
|   +-- Login/Default
|   +-- Login/Loading
|   +-- Login/Error
|   +-- Login/Validation
|   +-- Dashboard/Default
|   +-- Dashboard/Loading
|   +-- Dashboard/Empty
|   +-- Dashboard/Error
|   +-- CreateTask/Default
|   +-- CreateTask/Loading
|   +-- CreateTask/Error
|   +-- CreateTask/Validation
|   +-- TaskDetailEdit/Default
|   +-- TaskDetailEdit/Loading
|   +-- TaskDetailEdit/Error
|   +-- TaskDetailEdit/Validation
+-- 05_Prototype
|   +-- Flow 1: FL-001 Register Account (Success)
|   +-- Flow 2: FL-003 Login (Success)
|   +-- Flow 3: FL-005 Create Task (Success)
|   +-- Flow 4: FL-007 View and Filter Dashboard
|   +-- Flow 5: FL-008 Edit Task (Change Status to Completed)
|   +-- Flow 6: FL-009 Assign Task
|   +-- Flow 7: FL-010 Delete Task
+-- 06_Handoff
    +-- Token usage rules
    +-- Component guidelines
    +-- Responsive specs
    +-- Edge cases
    +-- Accessibility notes
```

---

## 14. Quality Checklist

### Pre-Export Validation
- [X] All screens have required states (Default/Loading/Empty/Error/Validation where applicable)
- [X] All components use design tokens (no hard-coded values)
- [X] Color contrast meets WCAG AA (>=4.5:1 text, >=3:1 UI)
- [X] Focus states defined for all interactive elements
- [X] Touch targets >= 44x44px (mobile)
- [X] Prototype flows wired and functional for key use cases
- [X] Naming conventions followed for layers, frames, and components
- [X] Export manifest complete

### Post-Generation
- [X] designsystem.md updated with Figma references
- [X] Export manifest generated
- [X] JPG files named correctly
- [X] Handoff documentation complete