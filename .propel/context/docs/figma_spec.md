# Figma Design Specification: TaskFlow

## 1. UI Impact Assessment (Derived from FR-XXX and UC-XXX)

This section identifies all functional requirements and use cases that have a direct user interface and user experience impact, outlining the necessary UI elements and interactions.

*   **FR-USER-001 (Register Account) & UC-001 (Register Account):**
    *   **UI:** Registration Form (Name, Email, Password, Confirm Password fields, "Register" button). Error messages, success message.
    *   **Interaction:** Input validation on form submission, password strength indication, redirection post-success.
*   **FR-USER-002 (Log In):**
    *   **UI:** Login Form (Email, Password fields, "Log In" button). Error messages (e.g., "Invalid credentials"), Forgot Password link (optional, but good practice).
    *   **Interaction:** Secure login flow, redirection to dashboard post-success.
*   **FR-TASK-001 (Create Task) & UC-002 (Create Task):**
    *   **UI:** "New Task" button. Task Creation Form (Title text input, Description textarea, Status dropdown, Priority dropdown, "Create Task" button). Success message.
    *   **Interaction:** Form submission, inline validation for mandatory title, immediate display of new task on dashboard.
*   **FR-TASK-002 (Assign Task):**
    *   **UI:** "Assign To" action/button on Task Detail or Dashboard card. User Search/Selection component (e.g., multi-select dropdown with search, or modal with user list). Confirmation modal.
    *   **Interaction:** Selecting users, confirming assignment, notification trigger.
*   **FR-TASK-003 (Modify Task):**
    *   **UI:** Task Detail/Edit View. Editable Title, Description, Status, Priority fields. "Save Changes" button, "Cancel" button. Validation feedback.
    *   **Interaction:** Updating task details, saving changes, immediate reflection on dashboard.
*   **FR-TASK-004 (Update Task Status to 'Completed'):**
    *   **UI:** Quick action element on task card/detail (e.g., checkbox, "Mark as Complete" button, or status dropdown with 'Completed' option).
    *   **Interaction:** Single click/selection to update status, visual change on UI.
*   **FR-TASK-005 (Delete Task):**
    *   **UI:** "Delete Task" action/button on task card/detail. Confirmation Dialog/Modal ("Are you sure?").
    *   **Interaction:** Deletion confirmation, task removal from UI.
*   **FR-DASH-001 (Main Dashboard Display):**
    *   **UI:** Main Dashboard layout. Task List (tabular or card-based display). Each task entry with Title, Status, Priority, Assignee(s). Navigation elements. "Create Task" button.
    *   **Interaction:** Loading state for tasks, displaying relevant tasks based on user.
*   **FR-DASH-002 (Filter Task Dashboard):**
    *   **UI:** Filter controls (e.g., tabs, dropdowns, checkboxes) for Status.
    *   **Interaction:** Filtering tasks based on selected status, immediate UI update.
*   **FR-NOTIF-001 (Task Assignment Notification):**
    *   **UI:** In-app Notification (e.g., Bell icon with badge, Toast notification). Email notification template (for Option B).
    *   **Interaction:** Automatic display of notification upon assignment, option to dismiss toast.

## 2. UX Requirements (UXR-XXX)

Derived from Functional Requirements (FR-XXX) and Use Cases (UC-XXX), these UXRs define the user-facing design goals.

*   **UXR-USER-001: Account Registration Form**
    *   **Description:** The user registration form SHALL clearly guide new users through the account creation process.
    *   **Acceptance Criteria:**
        *   User can easily locate and navigate to the registration page.
        *   All mandatory fields (Name, Email, Password, Confirm Password) are clearly labeled and visually distinct.
        *   Password requirements are displayed next to the password field (e.g., "Min 8 chars, 1 uppercase, 1 lowercase, 1 number, 1 symbol").
        *   Inline validation feedback is provided in real-time or upon field blur for format errors (e.g., invalid email, password mismatch, insufficient strength).
        *   An error message "This email is already taken" is displayed if a user attempts to register with an existing email.
        *   A success message "Account created successfully!" is displayed upon successful registration before redirection to login.
        *   The registration form is accessible via keyboard navigation.
*   **UXR-USER-002: Secure Login Experience**
    *   **Description:** The login experience SHALL be intuitive and provide clear feedback on authentication outcomes.
    *   **Acceptance Criteria:**
        *   User can easily locate and navigate to the login page.
        *   Email and Password fields are clearly labeled.
        *   "Log In" button is prominent.
        *   An "Invalid credentials" error message is displayed for incorrect email/password attempts, without distinguishing which field is incorrect.
        *   User is redirected to the main task dashboard upon successful login.
        *   The login form is accessible via keyboard navigation.
*   **UXR-TASK-001: Intuitive Task Creation**
    *   **Description:** Users SHALL be able to create new tasks quickly and efficiently with all necessary details.
    *   **Acceptance Criteria:**
        *   A "New Task" button or similar clear call-to-action is prominently available on the dashboard.
        *   The task creation form includes clearly labeled fields for Title (text input), Description (textarea), Status (dropdown, default 'Open'), and Priority (dropdown, default 'Medium').
        *   The "Task Title" field is marked as mandatory.
        *   Inline error message "Task Title is required" is displayed if the title is empty on submission.
        *   A success message "Task created successfully!" is displayed upon successful creation.
        *   The newly created task immediately appears on the user's dashboard without manual refresh.
*   **UXR-TASK-002: Streamlined Task Assignment**
    *   **Description:** Users SHALL be able to assign tasks to team members easily.
    *   **Acceptance Criteria:**
        *   An "Assign To" action is readily available from the task dashboard or task detail view.
        *   The assignment interface allows searching for registered users.
        *   Users can select one or more team members from a list for assignment.
        *   Confirmation of assignment is clear (e.g., selected names appear, success toast).
        *   Assigned tasks reflect the assignee(s) on the dashboard.
*   **UXR-TASK-003: Easy Task Modification**
    *   **Description:** Users SHALL be able to modify existing task details quickly and see updates reflected immediately.
    *   **Acceptance Criteria:**
        *   Clicking a task on the dashboard or an "Edit" action leads to a task detail/edit view.
        *   All editable fields (Title, Description, Status, Priority) are pre-filled with current task data.
        *   "Save Changes" and "Cancel" buttons are clearly visible.
        *   Inline error "Task Title is required" prevents saving with an empty title.
        *   Upon successful save, a success message "Task updated successfully!" is displayed, and changes are reflected on the dashboard.
*   **UXR-TASK-004: Quick Task Completion**
    *   **Description:** Users SHALL be able to mark a task as 'Completed' with minimal effort.
    *   **Acceptance Criteria:**
        *   A distinct visual control (e.g., checkbox, dedicated button) is available on the task card or detail view to mark as 'Completed'.
        *   Activating this control instantly updates the task's status to 'Completed'.
        *   The visual representation of the task on the dashboard immediately reflects its 'Completed' status (e.g., strikethrough, changed color).
*   **UXR-TASK-005: Safe Task Deletion**
    *   **Description:** Users SHALL be able to delete tasks with a clear confirmation step to prevent accidental data loss.
    *   **Acceptance Criteria:**
        *   A "Delete Task" action is available on the task card or detail view.
        *   Clicking "Delete Task" triggers a confirmation modal asking, "Are you sure you want to delete this task? This action cannot be undone." with "Delete" and "Cancel" buttons.
        *   Upon confirmation, the task is permanently removed from the dashboard and database.
        *   A success message "Task deleted successfully!" is displayed.
*   **UXR-DASH-001: Comprehensive Task Dashboard**
    *   **Description:** The main dashboard SHALL provide a clear and comprehensive overview of all relevant tasks for the logged-in user.
    *   **Acceptance Criteria:**
        *   The dashboard loads automatically after login.
        *   Tasks are displayed in a legible and scannable format (e.g., cards or a table).
        *   Each task entry visibly includes Title, Status, Priority, and Assignee(s).
        *   All tasks relevant to the user (created by, assigned to, or within the team) are displayed.
        *   The dashboard dynamically updates to reflect real-time task changes (creation, updates, deletion).
        *   A 'Loading' state is shown while tasks are being fetched.
        *   An 'Empty' state message is shown if no tasks are available.
        *   An 'Error' state message is shown if tasks fail to load.
*   **UXR-DASH-002: Effective Task Filtering**
    *   **Description:** Users SHALL be able to filter their task dashboard by status to quickly find specific tasks.
    *   **Acceptance Criteria:**
        *   Filtering controls for task statuses ('Open', 'In Progress', 'Completed', 'On Hold') are clearly visible and easy to use.
        *   Selecting a status filter immediately updates the task list to show only tasks matching that status.
        *   Multiple status filters can be applied simultaneously if desired (e.g., 'Open' and 'In Progress').
        *   Deselecting all filters reverts the dashboard to show all relevant tasks.
*   **UXR-NOTIF-001: Clear Assignment Notifications**
    *   **Description:** Users SHALL receive clear and timely notifications when a task is assigned to them.
    *   **Acceptance Criteria:**
        *   Upon task assignment, an in-app toast notification appears, stating "[Assigner Name] assigned you to '[Task Title]'." with a link to the task.
        *   Alternatively (or in addition), an email notification is sent with similar content.
        *   Notifications are dismissible.

## 3. Persona Analysis and User Journeys

### 3.1 Personas

**Persona 1: Alex, The Team Member**
*   **Demographics:** 28 years old, Junior Project Coordinator, works in a small marketing agency.
*   **Goals:**
    *   Easily see what tasks are assigned to him.
    *   Quickly update his task progress and status.
    *   Collaborate efficiently with colleagues.
    *   Avoid missing deadlines.
*   **Frustrations:**
    *   Tasks scattered across emails, Slack, and informal chats.
    *   Unclear accountability for specific tasks.
    *   Difficulty finding all his "to-do" items in one place.
    *   Cumbersome tools that require too many clicks.
*   **Quote:** "I just need a simple list of what I need to do, what's next, and a quick way to say I'm done."

**Persona 2: Sarah, The Team Manager**
*   **Demographics:** 35 years old, Creative Director, manages a team of 8 designers.
*   **Goals:**
    *   Gain clear visibility into team workload and progress.
    *   Effortlessly assign tasks and track completion.
    *   Identify potential bottlenecks early.
    *   Ensure team accountability.
    *   Reduce time spent on administrative task tracking.
*   **Frustrations:**
    *   Lack of a centralized overview of team tasks.
    *   Difficulty tracking who is responsible for what.
    *   Constant need to follow up on task statuses manually.
    *   Inefficient reporting on project progress.
*   **Quote:** "I need to know what everyone is working on, if we're on track, and who needs help, without being a micromanager."

### 3.2 User Journeys

**User Journey 1: Alex Registers and Creates His First Task**
1.  **Entry Point:** Alex hears about TaskFlow from a colleague.
2.  **Action: Register Account (UC-001)**
    *   Alex visits `/register`.
    *   He fills out Name, Email, Password, Confirm Password.
    *   He clicks "Register".
    *   **System:** Validates input, creates account, displays "Account created successfully!", redirects to `/login`.
3.  **Action: Log In (FR-USER-002)**
    *   Alex enters his new email and password.
    *   He clicks "Log In".
    *   **System:** Validates credentials, issues JWT, redirects to `/dashboard`.
4.  **Action: Create Task (UC-002)**
    *   Alex sees the "New Task" button on the dashboard.
    *   He clicks "New Task".
    *   **System:** Displays the task creation form.
    *   Alex enters "Design wireframes for marketing site" (Title), "Based on spec document V1.2" (Description), leaves Status as 'Open', sets Priority to 'High'.
    *   He clicks "Create Task".
    *   **System:** Validates input, creates task, displays "Task created successfully!", redirects to `/dashboard`.
5.  **Outcome:** Alex sees "Design wireframes for marketing site" proudly displayed on his dashboard.

**User Journey 2: Sarah Assigns and Tracks a Critical Task**
1.  **Entry Point:** Sarah logs into TaskFlow to review team progress.
2.  **Action: Log In (FR-USER-002)**
    *   Sarah navigates to `/login`, enters credentials, logs in successfully.
    *   **System:** Redirects to `/dashboard`.
3.  **Action: Create Task (UC-002)**
    *   Sarah clicks "New Task".
    *   She enters "Q3 Report Analysis" (Title), "Analyze market data for Q3" (Description), Status 'Open', Priority 'High'.
    *   She clicks "Create Task".
    *   **System:** Task created, displayed on dashboard.
4.  **Action: Assign Task (FR-TASK-002)**
    *   Sarah clicks on the "Q3 Report Analysis" task card.
    *   She sees an "Assign To" option.
    *   She searches for "Alex", selects him from the dropdown.
    *   She confirms the assignment.
    *   **System:** Updates assignment, triggers notification to Alex.
5.  **Action: View Dashboard (FR-DASH-001) & Filter (FR-DASH-002)**
    *   Sarah returns to her dashboard.
    *   She sees "Q3 Report Analysis" now assigned to "Alex".
    *   She uses the 'Status' filter to view only 'In Progress' tasks, monitoring ongoing work.
6.  **Outcome:** Sarah has successfully assigned a critical task and can now track its progress centrally.

## 4. Screen Inventory (Derived from Use Cases)

All screens are directly mapped to the defined use cases, ensuring no arbitrary screens are included.

1.  **`Auth/RegisterScreen`**
    *   **Purpose:** Allows new users to create an account.
    *   **Derived from:** UC-001 (Register Account)
2.  **`Auth/LoginScreen`**
    *   **Purpose:** Allows registered users to log into their account.
    *   **Derived from:** FR-USER-002 (Log In)
3.  **`Dashboard/MainDashboardScreen`**
    *   **Purpose:** Displays all relevant tasks to the logged-in user, serves as the central hub.
    *   **Derived from:** FR-DASH-001 (Main Dashboard Display), UXR-DASH-001, UXR-DASH-002
4.  **`Tasks/CreateTaskScreen`**
    *   **Purpose:** Provides a form for users to create new tasks.
    *   **Derived from:** UC-002 (Create Task), UXR-TASK-001
5.  **`Tasks/TaskDetailScreen`** (also serves as `Tasks/EditTaskScreen`)
    *   **Purpose:** Displays detailed information about a single task and allows for modification.
    *   **Derived from:** FR-TASK-003 (Modify Task), FR-TASK-002 (Assign Task), FR-TASK-004 (Update Status), FR-TASK-005 (Delete Task), UXR-TASK-003, UXR-TASK-002, UXR-TASK-004, UXR-TASK-005
6.  **`Modals/ConfirmationModal`**
    *   **Purpose:** Confirms critical user actions like task deletion.
    *   **Derived from:** FR-TASK-005 (Delete Task), UXR-TASK-005
7.  **`Notifications/ToastNotification`**
    *   **Purpose:** Provides transient feedback for user actions and assignments.
    *   **Derived from:** FR-NOTIF-001 (Task Assignment Notification), UXR-NOTIF-001, and general success/error messages.

## 5. Screen Specifications with 5 States

Detailed specifications for key screens, including Default, Loading, Empty, Error, and Validation states.

### 5.1 `Auth/RegisterScreen`

*   **Default State:**
    *   **Layout:** Centered form with "Register" heading.
    *   **Fields:** Name (text input), Email (email input), Password (password input), Confirm Password (password input).
    *   **Labels:** Clear labels above each input.
    *   **Helper Text:** "Password must be at least 8 characters long, contain one uppercase, one lowercase, one number, and one symbol" below Password field.
    *   **Button:** Primary "Register" button.
    *   **Links:** "Already have an account? Log In" link.
*   **Loading State:**
    *   **Interaction:** User clicks "Register" button.
    *   **Visual:** "Register" button changes to a loading spinner or "Registering..." text and becomes disabled. Form fields are disabled.
*   **Empty State:** *N/A for a form screen.*
*   **Error State:**
    *   **Interaction:** Server-side error (e.g., database unavailable).
    *   **Visual:** A red alert/toast message at the top of the form: "An unexpected error occurred during registration. Please try again later." Form fields enabled.
    *   **Interaction:** User attempts to register with existing email.
    *   **Visual:** Email input field shows a red border and an inline error message: "This email address is already in use. Please use a different one or log in."
*   **Validation State:**
    *   **Interaction:** User attempts to submit with invalid/missing fields.
    *   **Visual:**
        *   **Missing Name:** Name input with red border and inline error: "Name is required."
        *   **Invalid Email:** Email input with red border and inline error: "Please enter a valid email address."
        *   **Weak Password:** Password input with red border and inline error: "Password does not meet requirements."
        *   **Passwords Mismatch:** Confirm Password input with red border and inline error: "Passwords do not match."

### 5.2 `Auth/LoginScreen`

*   **Default State:**
    *   **Layout:** Centered form with "Log In" heading.
    *   **Fields:** Email (email input), Password (password input).
    *   **Labels:** Clear labels above each input.
    *   **Button:** Primary "Log In" button.
    *   **Links:** "Don't have an account? Register" link. "Forgot Password?" link (optional, for future scope but good to design for).
*   **Loading State:**
    *   **Interaction:** User clicks "Log In" button.
    *   **Visual:** "Log In" button changes to a loading spinner or "Logging In..." text and becomes disabled. Form fields disabled.
*   **Empty State:** *N/A for a form screen.*
*   **Error State:**
    *   **Interaction:** Invalid credentials entered.
    *   **Visual:** A red alert/toast message above the form: "Invalid credentials. Please try again." Email and Password fields remain enabled.
    *   **Interaction:** Server-side error.
    *   **Visual:** A red alert/toast message above the form: "An unexpected error occurred. Please try again later."
*   **Validation State:**
    *   **Interaction:** User attempts to submit with empty fields.
    *   **Visual:**
        *   **Missing Email:** Email input with red border and inline error: "Email is required."
        *   **Missing Password:** Password input with red border and inline error: "Password is required."

### 5.3 `Dashboard/MainDashboardScreen`

*   **Default State (Tasks Exist):**
    *   **Layout:** App header (Logo, User Avatar/Name, Logout button). Left sidebar (or top navigation for mobile) with "Dashboard" link, "Create Task" button. Main content area displaying tasks.
    *   **Elements:** Filter controls (e.g., tabs: All, Open, In Progress, Completed, On Hold). List of Task Cards.
    *   **Task Card (for each task):**
        *   Title (H3)
        *   Description snippet (body text)
        *   Status indicator (e.g., colored badge: Open, In Progress, Completed, On Hold)
        *   Priority indicator (e.g., colored text: Low, Medium, High)
        *   Assignee(s) (e.g., avatar/initials + name)
        *   Actions: "Mark as Complete" (checkbox/button), "Edit" icon, "Delete" icon.
*   **Loading State:**
    *   **Interaction:** Initial load after login, or applying a filter, or refreshing tasks.
    *   **Visual:** A full-screen spinner overlaying the content area, or skeletal loading cards/shimmer effect in place of task cards. Header and navigation remain visible.
*   **Empty State (No Tasks):**
    *   **Interaction:** User has no relevant tasks (e.g., new user, all tasks deleted).
    *   **Visual:** Main content area displays: "No tasks to display! Start by creating a new task." with a prominent "Create New Task" button. Header and navigation visible.
*   **Error State (Failed to Load Tasks):**
    *   **Interaction:** API call to fetch tasks fails.
    *   **Visual:** Main content area displays: "Failed to load tasks. Please try again later." with a "Retry" button. A toast notification might also appear: "Error fetching tasks." Header and navigation visible.
*   **Validation State:** *N/A for a dashboard, as it's primarily a display screen.*

### 5.4 `Tasks/CreateTaskScreen` (Modal or Dedicated Page)

*   **Default State:**
    *   **Layout:** (Assumed Modal for quick creation) Centered modal titled "Create New Task".
    *   **Fields:** Task Title (text input), Description (textarea), Status (dropdown, default 'Open'), Priority (dropdown, default 'Medium').
    *   **Labels:** Clear labels above each input.
    *   **Buttons:** "Create Task" (Primary), "Cancel" (Secondary).
*   **Loading State:**
    *   **Interaction:** User clicks "Create Task".
    *   **Visual:** "Create Task" button changes to a loading spinner or "Creating..." text and becomes disabled. Form fields disabled.
*   **Empty State:** *N/A for a form screen.*
*   **Error State:**
    *   **Interaction:** Server-side error during creation.
    *   **Visual:** A red toast message: "An unexpected error occurred while creating the task. Please try again." Form fields enabled.
*   **Validation State:**
    *   **Interaction:** User attempts to submit with an empty title.
    *   **Visual:** Task Title input with red border and inline error: "Task Title is required."

## 6. User Flows and Navigation

### 6.1 Main Navigation Structure

*   **Global Header:**
    *   Logo (TaskFlow) - links to `/dashboard`
    *   User Profile/Avatar (with dropdown for Logout, Settings - future)
    *   (Optional) Notifications Bell Icon
*   **Primary Action:**
    *   "New Task" button (prominently displayed in header or dashboard) - opens `CreateTaskScreen`

### 6.2 Key User Flows

**Flow 1: User Registration & First Login**
1.  **Start:** User lands on `Auth/RegisterScreen` (`/register`).
2.  **Action:** Fills form, clicks "Register".
3.  **System:** Validates, creates account.
4.  **End:** Success message, redirects to `Auth/LoginScreen` (`/login`).
5.  **Action:** Fills login form, clicks "Log In".
6.  **System:** Authenticates.
7.  **End:** Redirects to `Dashboard/MainDashboardScreen` (`/dashboard`).

**Flow 2: Creating a New Task**
1.  **Start:** User is on `Dashboard/MainDashboardScreen`.
2.  **Action:** Clicks "New Task" button.
3.  **System:** Displays `Tasks/CreateTaskScreen` (modal or full page).
4.  **Action:** Fills task details, clicks "Create Task".
5.  **System:** Validates, creates task.
6.  **End:** Success message, `Tasks/CreateTaskScreen` closes (or redirects), `Dashboard/MainDashboardScreen` updates with new task.

**Flow 3: Editing an Existing Task**
1.  **Start:** User is on `Dashboard/MainDashboardScreen`.
2.  **Action:** Clicks "Edit" icon on a Task Card or clicks on a Task Card to view details.
3.  **System:** Displays `Tasks/TaskDetailScreen` (`/tasks/:id`).
4.  **Action:** Modifies fields, clicks "Save Changes".
5.  **System:** Validates, updates task.
6.  **End:** Success message, `Tasks/TaskDetailScreen` updates, `Dashboard/MainDashboardScreen` reflects changes.

**Flow 4: Assigning a Task**
1.  **Start:** User is on `Tasks/TaskDetailScreen` or `Dashboard/MainDashboardScreen`.
2.  **Action:** Clicks "Assign To" option.
3.  **System:** Displays user selection interface (e.g., dropdown, modal).
4.  **Action:** Searches/selects user(s), confirms.
5.  **System:** Updates assignment.
6.  **End:** Success message, `Tasks/TaskDetailScreen` or `Dashboard/MainDashboardScreen` shows assignee(s), notification sent.

**Flow 5: Deleting a Task**
1.  **Start:** User is on `Dashboard/MainDashboardScreen` or `Tasks/TaskDetailScreen`.
2.  **Action:** Clicks "Delete" icon/button.
3.  **System:** Displays `Modals/ConfirmationModal`.
4.  **Action:** Clicks "Delete" within the modal.
5.  **System:** Deletes task.
6.  **End:** Success message, `Modals/ConfirmationModal` closes, task removed from `Dashboard/MainDashboardScreen`.

## 7. Component Mapping

This section maps the UI elements on specific screens to reusable Design System components.

*   **`Auth/RegisterScreen`:**
    *   Heading: `Typography/H2`
    *   Labels: `Typography/Label`
    *   Name, Email, Password, Confirm Password inputs: `Forms/TextInput` (password type for password fields)
    *   Helper Text: `Typography/HelperText`
    *   "Register" button: `Buttons/PrimaryButton`
    *   "Log In" link: `Buttons/LinkButton`
    *   Error messages: `Forms/InlineErrorMessage`, `Notifications/Toast`
*   **`Auth/LoginScreen`:**
    *   Heading: `Typography/H2`
    *   Labels: `Typography/Label`
    *   Email, Password inputs: `Forms/TextInput` (password type for password field)
    *   "Log In" button: `Buttons/PrimaryButton`
    *   "Register" link: `Buttons/LinkButton`
    *   Error messages: `Notifications/Toast`
*   **`Dashboard/MainDashboardScreen`:**
    *   App Header: `Navigation/AppHeader`
    *   "New Task" button: `Buttons/PrimaryButton`
    *   Filter tabs/buttons: `Navigation/FilterTabs` or `Buttons/SecondaryButton` (grouped)
    *   Task Cards: `Cards/TaskCard`
    *   Loading state: `Feedback/SkeletonLoader` or `Feedback/Spinner`
    *   Empty state message: `Feedback/EmptyState`
    *   Error state message: `Feedback/ErrorMessage`
    *   Toast Notifications: `Notifications/Toast`
*   **`Tasks/CreateTaskScreen`:**
    *   Modal container: `Modals/Modal`
    *   Modal Title: `Typography/H3`
    *   Labels: `Typography/Label`
    *   Task Title input: `Forms/TextInput`
    *   Description textarea: `Forms/Textarea`
    *   Status, Priority dropdowns: `Forms/Dropdown`
    *   "Create Task" button: `Buttons/PrimaryButton`
    *   "Cancel" button: `Buttons/SecondaryButton`
    *   Inline error: `Forms/InlineErrorMessage`
*   **`Tasks/TaskDetailScreen`:**
    *   Heading: `Typography/H1` (for task title)
    *   Description: `Typography/Body` (editable)
    *   Status, Priority indicators: `Indicators/StatusBadge`, `Indicators/PriorityText`
    *   Assignee(s) display: `Components/AvatarGroup` or `Components/UserChip`
    *   "Edit" button: `Buttons/SecondaryButton` (or implicit on fields)
    *   "Save Changes" button: `Buttons/PrimaryButton`
    *   "Cancel" button: `Buttons/SecondaryButton`
    *   "Assign To" selector: `Forms/SearchableDropdown` or `Modals/UserSelectionModal`
    *   "Mark as Complete" checkbox/button: `Forms/Checkbox` or `Buttons/SecondaryButton`
    *   "Delete Task" button: `Buttons/DestructiveButton`
*   **`Modals/ConfirmationModal`:**
    *   Modal container: `Modals/Modal`
    *   Modal Title: `Typography/H3`
    *   Confirmation Message: `Typography/Body`
    *   "Delete" button: `Buttons/DestructiveButton`
    *   "Cancel" button: `Buttons/SecondaryButton`

## 8. Interaction Patterns and Micro-animations

*   **Form Input Interaction:**
    *   **Focus:** Input fields gain a subtle border highlight (e.g., `color-primary-500`) on focus.
    *   **Validation:** Inline error messages appear immediately on blur or attempted submission for invalid inputs.
*   **Button States:**
    *   **Hover:** Buttons subtly lighten or darken on hover.
    *   **Active:** Buttons show a slightly pressed state.
    *   **Loading:** Loading spinner replaces button text.
*   **Task Card Interactions:**
    *   **Hover:** Task cards gain a subtle shadow or border highlight to indicate interactivity.
    *   **Click:** Clicking a task card navigates to `TaskDetailScreen`.
*   **Notifications (Toast):**
    *   **Entry/Exit:** Toast messages slide in from the top-right corner and fade out after a few seconds or on manual dismissal.
*   **Modal Overlay:**
    *   **Entry/Exit:** Modals fade in/out with a semi-transparent background overlay.
*   **Status Updates:**
    *   **"Mark as Complete":** Task card visually updates immediately (e.g., strikethrough, status badge color change) with a quick fade transition.

## 9. Accessibility Requirements (WCAG Compliance)

Adherence to WCAG 2.1 AA standards will be a priority for all UI components and flows.

*   **Perceivable:**
    *   **Color Contrast (1.4.3):** All text and interactive elements MUST meet a minimum contrast ratio of 4.5:1 against their background.
    *   **Non-text Content (1.1.1):** All images and non-text elements (e.g., icons, charts) MUST have appropriate alternative text (`alt` attributes) or be marked as decorative.
    *   **Visual Presentation (1.4.1):** Information conveyed by color MUST also be conveyed in another perceivable way (e.g., text label, icon).
*   **Operable:**
    *   **Keyboard Navigation (2.1.1):** All interactive elements (buttons, links, form fields, filters) MUST be reachable and operable using only a keyboard. A clear visual focus indicator MUST be provided.
    *   **No Keyboard Trap (2.1.2):** Users MUST be able to navigate away from any interactive element using only the keyboard.
    *   **Timing Adjustable (2.2.1):** If any time limits are imposed, users MUST be able to adjust, extend, or turn off the limits (N/A for MVP, but consider for future notification timeouts).
    *   **Focus Order (2.4.3):** Keyboard focus MUST move in a logical and predictable order (e.g., top-to-bottom, left-to-right within a section).
    *   **Link Purpose (In Context) (2.4.4):** The purpose of each link MUST be clear from its text or context.
*   **Understandable:**
    *   **Language of Page (3.1.1):** The human language of the page MUST be identified (e.g., `lang="en"`).
    *   **Labels or Instructions (3.3.2):** All form fields MUST have visible and programmatically associated labels. Clear instructions for input (e.g., password requirements) MUST be provided.
    *   **Error Identification (3.3.1):** When an input error is automatically detected, the item in error MUST be identified, and the error described to the user in text.
    *   **Error Suggestion (3.3.3):** If an input error is detected and suggestions for correction are known, then the suggestions MUST be provided to the user.
*   **Robust:**
    *   **Parsing (4.1.1):** Markup MUST be well-formed and valid HTML.
    *   **Name, Role, Value (4.1.2):** All UI components MUST have proper programmatic names, roles, and states (e.g., ARIA attributes for custom components like dropdowns).

## 10. Responsive Design Breakpoints

Based on NFR-USABILITY-001, the UI will be responsive for desktop and tablet screen sizes (768px and above).

*   **`xs` (Extra Small / Mobile-first base):** < 640px (Implicit, as a mobile-first approach is generally best practice, though explicit breakpoint starts higher).
*   **`sm` (Small / Tablet Portrait):** >= 640px
*   **`md` (Medium / Tablet Landscape):** >= 768px (Primary tablet target from NFR)
*   **`lg` (Large / Desktop):** >= 1024px
*   **`xl` (Extra Large / Large Desktop):** >= 1280px
*   **`2xl` (Double Extra Large / Ultra-wide Desktop):** >= 1536px

**Layout Adaptations:**
*   **Header:** Stays fixed at the top, potentially collapsing navigation items into a hamburger menu for `sm` and `xs` (if `xs` is considered).
*   **Dashboard:**
    *   **`md` and up:** Multi-column layout for task cards or a wider table view. Filters are visible horizontally.
    *   **`sm` and `xs`:** Single-column layout for task cards. Filters may collapse into a dropdown or bottom sheet.
*   **Forms:** Stacked labels and inputs for smaller screens, side-by-side for larger screens where appropriate.
*   **Modals:** Full-screen on `sm` and `xs`, centered and fixed-width on `md` and up.

---