# Figma Design Specification: TaskFlow

## 1. UI Impact Assessment

This section details the direct UI/UX implications of each functional and non-functional requirement from the Product Specification.

*   **FR-USER-001 (Register Account):** Requires a dedicated **Registration Form** screen with input fields (Name, Email, Password, Confirm Password), submission button, and clear validation/error messaging. Implies a successful redirection state.
*   **FR-USER-002 (Log In):** Requires a dedicated **Login Form** screen with input fields (Email, Password), submission button, and clear validation/error messaging. Implies a successful redirection state to the Dashboard.
*   **FR-TASK-001 (Create Task):** Requires a **Create Task Form** screen/modal accessible from the Dashboard, including input fields (Title, Description), dropdowns (Status, Priority), and a submission button. Success message and dashboard update.
*   **FR-TASK-002 (Assign Task):** Requires an **Assign Task** interaction within a Task Detail/Edit View or as a contextual action from the Dashboard. This will involve a searchable user selection component (e.g., multi-select dropdown or modal).
*   **FR-TASK-003 (Modify Task):** Requires a **Task Detail/Edit View** with pre-filled, editable fields (Title, Description, Status, Priority), a "Save Changes" button, and validation/error handling.
*   **FR-TASK-004 (Update Task Status to 'Completed'):** Requires a prominent and easy-to-access action (e.g., checkbox, button, or dropdown option) on the Dashboard task list item and within the Task Detail View to mark a task as 'Completed'.
*   **FR-TASK-005 (Delete Task):** Requires a "Delete Task" action on the Dashboard task list item and within the Task Detail View, coupled with a **Confirmation Dialog** for user confirmation.
*   **FR-DASH-001 (Main Dashboard):** Requires the primary **Dashboard Screen** displaying a list of tasks. Each task item must show Title, Status, Priority, and Assignee(s). This screen will handle various states (loading, empty, error).
*   **FR-DASH-002 (Filter Dashboard):** Requires **Filter Controls** (e.g., dropdowns, checkboxes, or tabs) on the Dashboard Screen to filter tasks by status.
*   **FR-NOTIF-001 (Task Assignment Notification):** Requires an **In-App Notification** mechanism (e.g., a toast message or a persistent notification badge/feed) to alert users of new task assignments.
*   **NFR-USABILITY-001 (Responsive UI):** All aforementioned UI components and screens MUST be designed to be fully responsive and adapt gracefully across desktop and tablet screen sizes (768px and above). This impacts layout, typography, spacing, and component sizing.

## 2. UX Requirements (UXR-XXX)

| ID           | Requirement                                                                                                                              | Acceptance Criteria                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| :----------- | :--------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **UXR-AUTH-001** | Users SHALL be able to easily create a new account.                                                                                      | 1. The registration form fields are clearly labeled and intuitive to fill. <br/> 2. Real-time inline validation is provided for email format, password strength, and password matching during input. <br/> 3. Error messages for duplicate emails or invalid input are explicit and user-friendly. <br/> 4. Upon successful registration, the user is clearly informed and seamlessly redirected to the login page. |
| **UXR-AUTH-002** | Users SHALL be able to securely log in to their account.                                                                                 | 1. The login form is simple, featuring email and password fields. <br/> 2. Feedback for invalid credentials is immediate and clear, without exposing whether the email or password was incorrect. <br/> 3. Users can submit the form using both mouse click and keyboard (Enter key).                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **UXR-TASK-001** | Users SHALL be able to efficiently create new tasks.                                                                                     | 1. The "New Task" button is prominently placed on the dashboard. <br/> 2. The task creation form is concise and clearly distinguishes mandatory (Title) from optional (Description) fields. <br/> 3. Default values for Status ('Open') and Priority ('Medium') are pre-selected to minimize input effort. <br/> 4. A success confirmation (e.g., toast message) is displayed, and the new task is immediately visible on the dashboard without requiring a page refresh.                                                                                                                                                                                                                                                                  |
| **UXR-TASK-002** | Users SHALL be able to clearly see and manage their tasks.                                                                               | 1. The dashboard provides a clear, scannable list of tasks, with each task displaying its title, status, priority, and assigned users. <br/> 2. Filtering tasks by status is intuitive and immediately updates the displayed list. <br/> 3. Actions like 'Edit', 'Assign', 'Complete', and 'Delete' are readily accessible for each task, either directly on the task item or via a detail view. |
| **UXR-TASK-003** | Users SHALL be able to update task details without friction.                                                                             | 1. The task detail/edit view presents current task information pre-filled in editable fields. <br/> 2. Changes can be saved with a clear "Save Changes" action, with appropriate feedback (e.g., success message). <br/> 3. Mandatory fields (Title) are validated upon submission, showing inline error messages for missing input.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **UXR-TASK-004** | Users SHALL be clearly informed when a task is assigned to them.                                                                         | 1. Upon task assignment, the assignee receives an immediate in-app notification (e.g., a non-intrusive toast message or a badge indicator on a notification icon). <br/> 2. The notification content clearly identifies the task and the assigner. <br/> 3. Notifications are actionable, allowing users to navigate directly to the assigned task.                                                                                                                                                                                                                                                                                                                                                                                        |
| **UXR-SYS-001** | The application SHALL be consistently responsive across supported devices (desktop, tablet).                                               | 1. All interactive elements (buttons, forms, navigation) remain easily tappable/clickable and appropriately sized on tablet devices (768px+). <br/> 2. Text and image content scales legibly across all supported screen sizes. <br/> 3. No horizontal scrolling is required on any screen within the specified breakpoints. <br/> 4. Layouts adjust intelligently to maximize screen real estate while maintaining readability and usability (e.g., collapsing navigation, reordering columns).                                                                                                                                                                                                                         |
| **UXR-FEED-001** | Users SHALL receive immediate and clear feedback for all major actions (e.g., creation, update, deletion, assignment).                   | 1. All form submissions (registration, login, create task, edit task) display a loading indicator while processing. <br/> 2. Successful operations are confirmed with a brief, positive toast notification. <br/> 3. Error conditions are communicated with clear, actionable messages, distinguishing between user input errors (inline validation) and system errors (generic error message). <br/> 4. Deletion actions prompt a confirmation dialog to prevent accidental data loss.                                                                                                                                                                                                                                 |

## 3. Persona Analysis and User Journeys

### 3.1 Personas

**Persona 1: Alex - The Dedicated Team Member**

*   **About Alex:** Alex is a software developer in a 5-person team. He's technically proficient but prefers tools that are straightforward and don't get in the way of his actual work. He often juggles multiple coding tasks and participates in daily stand-ups.
*   **Goals:**
    *   Easily see what tasks are assigned to him.
    *   Quickly update the status of his tasks (e.g., to 'In Progress' or 'Completed').
    *   Create personal tasks or tasks for his own work.
    *   Collaborate with others without heavy administrative overhead.
*   **Pain Points:**
    *   Tasks getting lost in email threads or chat messages.
    *   Unclear ownership of tasks.
    *   Difficulty seeing the overall progress of his assigned work.
*   **Motivations:** Efficiency, clear direction, personal accountability.
*   **FRs/UCs Most Relevant:** FR-TASK-001, FR-TASK-003, FR-TASK-004, FR-DASH-001, FR-DASH-002, FR-NOTIF-001.

**Persona 2: Sarah - The Project-Oriented Team Manager**

*   **About Sarah:** Sarah manages Alex's team and oversees a couple of small projects. She is responsible for distributing work, tracking progress, and ensuring projects stay on schedule. She values visibility and the ability to quickly identify bottlenecks.
*   **Goals:**
    *   Assign tasks to team members effectively.
    *   Get a quick overview of all ongoing tasks across the team.
    *   Track individual team member's progress on tasks.
    *   Create and edit tasks for the team.
    *   Ensure clear accountability for every task.
*   **Pain Points:**
    *   Lack of a centralized task overview.
    *   Difficulty knowing who is working on what.
    *   Time-consuming manual tracking through spreadsheets.
*   **Motivations:** Team productivity, project oversight, accountability, efficiency.
*   **FRs/UCs Most Relevant:** FR-TASK-001, FR-TASK-002, FR-TASK-003, FR-TASK-005, FR-DASH-001, FR-DASH-002.

### 3.2 User Journeys

**Journey 1: Alex (Team Member) - Completing an Assigned Task**

*   **Scenario:** Alex logs in to start his workday and sees a task assigned to him. He works on it and completes it.
*   **Flow:**
    1.  **Login:** Alex navigates to TaskFlow, enters credentials (FR-USER-002).
    2.  **View Dashboard:** System displays Dashboard (FR-DASH-001). Alex sees a notification for a newly assigned task (FR-NOTIF-001).
    3.  **Identify Task:** Alex locates the task "Implement User Profile API" on his dashboard, potentially using filters (FR-DASH-002) to see 'In Progress' tasks.
    4.  **Update Status:** Alex clicks on the task or an action button to mark it as 'In Progress' (initial update, then later to 'Completed'). (FR-TASK-003, FR-TASK-004)
    5.  **Task Completion:** After finishing, Alex marks the task as 'Completed' directly from the dashboard view or task detail. (FR-TASK-004)
    6.  **Confirmation:** System provides immediate visual feedback (e.g., status change, toast notification).

**Journey 2: Sarah (Team Manager) - Assigning and Tracking a New Task**

*   **Scenario:** Sarah needs to assign a new marketing brief to Alex and track its progress.
*   **Flow:**
    1.  **Login:** Sarah navigates to TaskFlow, enters credentials (FR-USER-002).
    2.  **View Dashboard:** System displays Dashboard (FR-DASH-001).
    3.  **Create New Task:** Sarah clicks "New Task" button (FR-TASK-001).
    4.  **Fill Task Details:** Sarah enters "Write Marketing Brief" as title, adds a description, sets priority to 'High'. (FR-TASK-001)
    5.  **Assign Task:** Sarah uses the "Assign To" feature, searches for "Alex", and selects him. (FR-TASK-002)
    6.  **Save Task:** Sarah saves the task. System confirms creation and assignment, sending a notification to Alex. (FR-TASK-001, FR-NOTIF-001)
    7.  **Track Task:** Sarah views the Dashboard (FR-DASH-001), filters by 'High' priority (FR-DASH-002), and checks the status of "Write Marketing Brief", seeing it's assigned to Alex.

## 4. Screen Inventory

All screens are derived from the Use Case Analysis and Functional Requirements.

1.  **Registration Screen** (UC-001, FR-USER-001)
2.  **Login Screen** (FR-USER-002)
3.  **Dashboard Screen** (FR-DASH-001, FR-DASH-002)
    *   Includes Task List
    *   Includes Filter Controls
    *   Includes "New Task" button
4.  **Create Task Screen (Modal/Page)** (UC-002, FR-TASK-001)
5.  **Task Detail / Edit Screen** (FR-TASK-002, FR-TASK-003, FR-TASK-004, FR-TASK-005)
    *   Includes Task Details (Title, Description, Status, Priority)
    *   Includes "Assign To" component
    *   Includes "Mark as Completed" action
    *   Includes "Delete Task" action
6.  **Confirmation Dialog (for Delete Task)** (FR-TASK-005)
7.  **Toast Notification** (FR-NOTIF-001, UXR-FEED-001)
8.  **Loading Overlay/Indicators** (UXR-FEED-001)

## 5. Screen Specifications

### 5.1 Registration Screen

*   **Purpose:** Allow new users to create an account.
*   **Key Elements:** App logo/name, "Register" heading, Name input, Email input, Password input, Confirm Password input, "Register" button, "Already have an account? Log in" link.
*   **States:**
    *   **Default:** Empty input fields, "Register" button enabled.
        *   _Layout:_ Centered form, clear field labels.
    *   **Loading:** "Register" button displays a spinner/loading state and is disabled. Input fields are disabled.
        *   _Visual cue:_ Spinner within button, reduced opacity on form.
    *   **Empty:** Not applicable for an entire screen, but individual fields can be empty.
    *   **Error (Server-side):** Generic error message (e.g., "An unexpected error occurred. Please try again.") displayed above the form or as a toast. "Register" button returns to default state.
        *   _Trigger:_ AF-1.3
    *   **Validation (Client-side/Inline):**
        *   _Invalid Email:_ "Please enter a valid email address."
        *   _Email Taken:_ "This email address is already in use." (AF-1.1)
        *   _Password Mismatch:_ "Passwords do not match."
        *   _Weak Password:_ "Password must be at least 8 characters, with 1 uppercase, 1 lowercase, 1 number, and 1 symbol."
        *   _Empty Mandatory Field:_ "This field is required."
        *   _Visual cue:_ Red border around field, red text error message below field.

### 5.2 Login Screen

*   **Purpose:** Allow registered users to authenticate and access the application.
*   **Key Elements:** App logo/name, "Login" heading, Email input, Password input, "Log In" button, "Don't have an account? Register" link.
*   **States:**
    *   **Default:** Empty input fields, "Log In" button enabled.
        *   _Layout:_ Centered form, clear field labels.
    *   **Loading:** "Log In" button displays a spinner/loading state and is disabled. Input fields are disabled.
        *   _Visual cue:_ Spinner within button, reduced opacity on form.
    *   **Empty:** Not applicable for an entire screen.
    *   **Error (Invalid Credentials):** "Invalid credentials. Please check your email and password." message displayed above the form or as a toast. "Log In" button returns to default state.
        *   _Trigger:_ FR-USER-002, AC 3
    *   **Validation (Client-side/Inline):**
        *   _Empty Email/Password:_ "This field is required."
        *   _Visual cue:_ Red border around field, red text error message below field.

### 5.3 Dashboard Screen

*   **Purpose:** Display a comprehensive list of relevant tasks and provide navigation/filtering options.
*   **Key Elements:** App header (Logo, User Avatar/Name, "New Task" button), Task list display area, Filter controls (by Status), Task items.
*   **States:**
    *   **Default:** Task list populated with tasks, filters active or showing 'All'.
        *   _Layout:_ Header, filter section, main content area with task cards/table.
    *   **Loading:** A full-screen or section-specific spinner/skeleton loader is displayed while tasks are fetched.
        *   _Visual cue:_ Skeleton cards/rows, central spinner.
    *   **Empty:** No tasks to display based on current filters or for the user. A prominent message: "No tasks found! Start by creating a new task or enjoy your free time." with a clear "Create New Task" button.
        *   _Visual cue:_ Illustration, friendly message.
    *   **Error:** Failed to load tasks from the server. Message: "Failed to load tasks. Please try again later." with a "Retry" button.
        *   _Visual cue:_ Error icon, distinct styling for error message.
    *   **Validation:** Not applicable for the entire dashboard display, but filters could have implicit validation (e.g., selecting invalid options, though unlikely with dropdowns).

### 5.4 Create Task Screen (Modal or dedicated page)

*   **Purpose:** Facilitate the creation of new tasks.
*   **Key Elements:** "Create New Task" heading, Title input, Description textarea, Status dropdown ('Open', 'In Progress', 'Completed', 'On Hold'), Priority dropdown ('Low', 'Medium', 'High'), "Save Task" button, "Cancel" button.
*   **States:**
    *   **Default:** Empty title and description, Status defaults to 'Open', Priority defaults to 'Medium'. "Save Task" button enabled.
        *   _Layout:_ Form with clearly labeled fields.
    *   **Loading:** "Save Task" button displays a spinner and is disabled. Input fields are disabled.
        *   _Visual cue:_ Spinner within button.
    *   **Empty:** Not applicable for an entire screen.
    *   **Error (Server-side):** Generic error message (e.g., "An unexpected error occurred. Please try again.") displayed as a toast or above the form.
    *   **Validation (Client-side/Inline):**
        *   _Empty Title:_ "Task Title is required."
        *   _Visual cue:_ Red border around title input, red text error message below.

### 5.5 Task Detail / Edit Screen

*   **Purpose:** View, modify, assign, mark complete, or delete an existing task.
*   **Key Elements:** Task title (editable), Description (editable textarea), Status dropdown (editable), Priority dropdown (editable), Assign To component (searchable user list), "Mark as Completed" button/checkbox, "Save Changes" button, "Delete Task" button, "Back to Dashboard" link.
*   **States:**
    *   **Default:** Task details pre-filled in editable fields, all action buttons enabled.
        *   _Layout:_ Task information section, assignment section, action buttons.
    *   **Loading:** While fetching task details: skeleton loader for the screen. While saving/assigning/deleting: affected buttons/sections show spinners and are disabled.
    *   **Empty:** Not applicable for an existing task, implies an Error state if task ID is invalid.
    *   **Error (Server-side):** Failed to load task details ("Task not found or you don't have permission.") or failed to save changes ("An unexpected error occurred while saving changes.").
    *   **Validation (Client-side/Inline):**
        *   _Empty Title (on update):_ "Task Title is required."
        *   _Visual cue:_ Red border around title input, red text error message below.

### 5.6 Confirmation Dialog (for Delete Task)

*   **Purpose:** Prevent accidental deletion of tasks.
*   **Key Elements:** Modal overlay, "Confirm Deletion" heading, "Are you sure you want to delete '[Task Title]'? This action cannot be undone." message, "Delete" button (destructive style), "Cancel" button.
*   **States:**
    *   **Default:** Dialog is visible, buttons enabled.
    *   **Loading:** "Delete" button shows spinner and is disabled. "Cancel" button disabled.
    *   **Empty:** Not applicable.
    *   **Error:** Not applicable (error feedback would be a toast outside the dialog).
    *   **Validation:** Not applicable.

## 6. User Flows and Navigation

### 6.1 Global Navigation

*   **App Header:** Always present when logged in.
    *   `[TaskFlow Logo]` - Navigates to Dashboard.
    *   `[User Avatar / Name]` - Placeholder for future profile/settings, currently no explicit profile features in MVP.
    *   `[Notification Icon (Bell)]` - Badge indicator for unread notifications (FR-NOTIF-001).
    *   `[New Task Button]` - Global action to create a new task (FR-TASK-001).

### 6.2 Primary User Flows

1.  **Authentication Flow:**
    *   User (Unauthenticated) -> `Registration Screen` (UC-001, FR-USER-001)
        *   Success -> `Login Screen` (FR-USER-002)
        *   Error -> `Registration Screen` (inline errors, server error)
    *   User (Unauthenticated) -> `Login Screen` (FR-USER-002)
        *   Success -> `Dashboard Screen` (FR-DASH-001)
        *   Error -> `Login Screen` (invalid credentials)

2.  **Task Creation Flow:**
    *   User (Authenticated, on Dashboard) -> Click `[New Task Button]` -> `Create Task Screen (Modal/Page)` (UC-002, FR-TASK-001)
        *   Fill form, Click `[Save Task]`
        *   Success -> `Dashboard Screen` (with new task, toast notification)
        *   Error -> `Create Task Screen` (inline errors, server error)

3.  **Task Management Flow (from Dashboard):**
    *   User (Authenticated, on Dashboard) -> `Dashboard Screen` (FR-DASH-001, FR-DASH-002)
        *   Interact with `Filter Controls` (FR-DASH-002) -> Task List updates dynamically.
        *   Click `[Task Item]` -> `Task Detail / Edit Screen` (FR-TASK-003, FR-TASK-002, FR-TASK-004, FR-TASK-005)
        *   Click `[Mark as Completed]` (on task item directly) -> `Dashboard Screen` (task status updates, toast notification)
        *   Click `[Delete Task]` (on task item directly) -> `Confirmation Dialog` -> `Dashboard Screen` (task removed, toast notification)

4.  **Task Detail & Edit Flow:**
    *   User (Authenticated, on Task Detail / Edit Screen)
        *   Edit `[Title/Description/Status/Priority]` (FR-TASK-003)
        *   Click `[Assign To]` (FR-TASK-002) -> `User Selection Component`
        *   Click `[Mark as Completed]` (FR-TASK-004)
        *   Click `[Save Changes]`
            *   Success -> `Dashboard Screen` (updated task, toast notification)
            *   Error -> `Task Detail / Edit Screen` (inline errors, server error)
        *   Click `[Delete Task]` (FR-TASK-005) -> `Confirmation Dialog`
            *   Confirm -> `Dashboard Screen` (task removed, toast notification)
            *   Cancel -> `Task Detail / Edit Screen`

## 7. Component Mapping

This table outlines which core UI components are used on each screen.

| Screen / Feature          | Input Text Field | Textarea | Select Dropdown | Button   | Card   | Toast Notification | Spinner | Modal/Dialog | Badge/Tag |
| :------------------------ | :--------------- | :------- | :-------------- | :------- | :----- | :----------------- | :------ | :----------- | :-------- |
| **Registration Screen**   | Yes              | No       | No              | Yes      | No     | Yes                | Yes     | No           | No        |
| **Login Screen**          | Yes              | No       | No              | Yes      | No     | Yes                | Yes     | No           | No        |
| **Dashboard Screen**      | No               | No       | Yes (Filters)   | Yes      | Yes    | Yes                | Yes     | No           | Yes       |
| **Create Task Screen**    | Yes              | Yes      | Yes             | Yes      | No     | Yes                | Yes     | Yes          | No        |
| **Task Detail/Edit Screen** | Yes              | Yes      | Yes             | Yes      | No     | Yes                | Yes     | Yes          | Yes       |
| **Confirmation Dialog**   | No               | No       | No              | Yes      | No     | No                 | Yes     | Yes          | No        |
| **Notification Display**  | No               | No       | No              | No       | No     | Yes                | No      | No           | Yes       |

## 8. Interaction Patterns and Micro-animations

*   **Form Field Interaction:**
    *   **Focus State:** Input fields highlight with a distinct border color/shadow upon focus.
    *   **Validation Feedback:** Inline error messages appear immediately on blur or attempted submission.
*   **Button Interaction:**
    *   **Hover State:** Slight background color change or subtle shadow lift.
    *   **Active State:** Slight press-down effect or darker background.
    *   **Loading State:** Integrated spinner within the button, disabling interaction.
*   **Link Interaction:** Underline on hover, color change.
*   **Toast Notifications:** Slide in from top/bottom, briefly display, then slide out. Auto-dismiss after 3-5 seconds, or on user click.
*   **Modal/Dialogs:** Fade-in overlay with a slight scale-up animation for the dialog box.
*   **Task List Updates:**
    *   **Adding/Removing:** New tasks can gently fade/slide in. Deleted tasks can fade/slide out.
    *   **Status Changes:** Subtle background flash or color transition on the task card when its status is updated.
*   **Filtering:** Smooth transition of tasks as they appear/disappear from the list, rather than an abrupt change.

## 9. Accessibility Requirements (WCAG Compliance)

All UI components and screens will adhere to WCAG 2.1 Level AA.

*   **Perceivable:**
    *   **Color Contrast (1.4.3):** All text and interactive elements will meet a minimum contrast ratio of 4.5:1 against their background. Large text will meet 3:1.
    *   **Non-text Content (1.1.1):** All images and non-text elements will have appropriate `alt` text or be marked as decorative.
    *   **Resizable Text (1.4.4):** Users can resize text up to 200% without loss of content or functionality.
*   **Operable:**
    *   **Keyboard Navigation (2.1.1):** All interactive elements (buttons, links, form fields, filters) are reachable and operable via keyboard alone (Tab, Enter, Space keys).
    *   **Focus Indicator (2.4.7):** A clear visual focus indicator (e.g., outline, highlight) is visible for all interactive elements when navigated via keyboard.
    *   **No Keyboard Traps (2.1.2):** Users can easily navigate away from any component using only the keyboard.
    *   **Clear Labels (2.4.6, 3.3.2):** All form fields have visible, programmatically associated labels.
    *   **Error Identification (3.3.1):** Error messages clearly identify input errors and suggest corrections.
    *   **Descriptive Headings (2.4.6):** Page and section headings are descriptive and convey hierarchy.
*   **Understandable:**
    *   **Predictable Navigation (3.2.3):** Navigation elements appear in a consistent location and order across screens.
    *   **Language of Page (3.1.1):** The primary language of each web page is identified programmatically.
*   **Robust:**
    *   **Parsing (4.1.1):** HTML is well-formed and valid, ensuring compatibility with assistive technologies.
    *   **Name, Role, Value (4.1.2):** All custom UI components convey their name, role, and state to assistive technologies using appropriate HTML semantics and ARIA attributes (e.g., `aria-label`, `aria-describedby`, `role`).

## 10. Responsive Design Breakpoints

Based on NFR-USABILITY-001, the UI must be optimized for tablet and desktop screen sizes (768px and above).

*   **Mobile (Implicit/Fallback):** < 768px (Not explicitly optimized per NFR, but a basic fluid layout should handle it).
*   **Tablet Portrait (TP):** 768px - 1023px
    *   _Layout Adjustments:_ Main navigation might collapse into a hamburger menu or be simplified. Task lists might switch from multiple columns to a single column, or card views might stack. Forms would be full width.
*   **Tablet Landscape (TL) / Small Desktop (SD):** 1024px - 1279px
    *   _Layout Adjustments:_ Navigation might expand to a sidebar or remain as a top bar. Task lists could use 2-column grids. More complex components (e.g., Assign To selector) might have more space.
*   **Desktop (D):** 1280px - 1920px (and above)
    *   _Layout Adjustments:_ Standard desktop layouts with full navigation, multi-column task views, generous spacing. UI elements are optimized for mouse interaction.

The design will primarily focus on the 'Desktop' and 'Tablet' ranges, ensuring all elements are functional and aesthetically pleasing within these. Mobile will be a best-effort, fluid layout approach if not explicitly designed for.