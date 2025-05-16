# Feedback Modal Implementation

## Description
Tasks for implementing the user feedback modal feature.

## Completed Tasks
<!-- Add completed tasks here, e.g., - [x] Task Name (Completed YYYY-MM-DD) -->

## In Progress Tasks
<!-- Add tasks currently being worked on here, e.g., - [ ] Task Name -->

## Future Tasks
- [ ] **Task 1: Basic Modal Structure Setup**
    - [ ] Create a new React component for the feedback modal (e.g., `FeedbackModal.tsx`).
    - [ ] Implement the main trigger button "Ihr Feedback" fixed to the bottom right of the screen.
    - [ ] Use Radix UI `Dialog.Root`, `Dialog.Trigger`, `Dialog.Portal`, `Dialog.Overlay`, `Dialog.Content`, `Dialog.Title`, `Dialog.Description`, `Dialog.Close` for the basic modal structure.
    - [ ] Set a basic title like "Geben Sie uns Ihr Feedback".
- [ ] **Task 2: Form Implementation within Modal**
    - [ ] Integrate Radix UI `Form.Root` for form management.
    - [ ] Add Radix UI `Form.Field` for the feedback text area:
        - [ ] Use Radix UI `Form.Label` ("Ihr Feedback:").
        - [ ] Use Radix UI `TextArea` or `Form.Control` as="textarea".
        - [ ] Make this field mandatory.
        - [ ] Add placeholder "Ihr Feedback...".
    - [ ] Add Radix UI `Form.Field` for the optional name input:
        - [ ] Use Radix UI `Form.Label` ("Name (optional):").
        - [ ] Use Radix UI `TextField.Input` or `Form.Control` as="input" type="text".
        - [ ] Add placeholder "Ihr Name".
    - [ ] Add Radix UI `Form.Field` for the optional email input:
        - [ ] Use Radix UI `Form.Label` ("E-Mail (optional):").
        - [ ] Use Radix UI `TextField.Input` or `Form.Control` as="input" type="email".
        - [ ] Add placeholder "Ihre E-Mail-Adresse".
    - [ ] Add a "Senden" (Submit) button using Radix UI `Form.Submit` or `Button`.
    - [ ] Add a "Schließen" (Close) button or 'X' icon using Radix UI `Dialog.Close`.
- [ ] **Task 3: Client-Side Validation Logic**
    - [ ] Implement validation for the feedback text area (must not be empty, UC-004).
        - [ ] Use Radix UI `Form.Message` to display error "Bitte geben Sie Ihr Feedback ein."
    - [ ] Implement validation for the email input (must be a valid email format if provided, UC-005).
        - [ ] Use Radix UI `Form.Message` to display error "Bitte geben Sie eine gültige E-Mail-Adresse ein oder lassen Sie das Feld leer."
    - [ ] Ensure the "Senden" button is appropriately enabled/disabled based on validation state or form prevents submission.
- [ ] **Task 4: Form Submission Handling (Client-Side)**
    - [ ] On "Senden" click, after passing client-side validation:
        - [ ] Simulate API call (e.g., `console.log` feedback data, or use a mock function).
        - [ ] Display a loading state (e.g., disable submit button, show spinner).
    - [ ] **Success State (UC-001, UC-002):**
        - [ ] Hide loading state.
        - [ ] Display a success message (e.g., "Vielen Dank für Ihr Feedback!" within the modal or as a toast).
        - [ ] Clear all form fields.
        - [ ] Optionally, auto-close modal after a short delay.
    - [ ] **Error State (UC-006 - Simulate Client-Side for now):**
        - [ ] Hide loading state.
        - [ ] Display an error message (e.g., "Ihr Feedback konnte nicht gesendet werden. Bitte versuchen Sie es später erneut.").
        - [ ] Preserve user input in the form fields.
- [ ] **Task 5: State Management for Modal and Form**
    - [ ] Manage modal open/close state (e.g., using `React.useState`).
    - [ ] Manage form input values (e.g., using `React.useState` for each field or a single state object).
    - [ ] Manage loading and error states for submission.
    - [ ] Ensure form fields are reset when the modal is closed and reopened (UC-003).
- [ ] **Task 6: Styling and Theming**
    - [ ] Apply basic styling to the trigger button (positioning, appearance).
    - [ ] Style the modal content (layout, spacing, typography) to be clean and user-friendly.
    - [ ] Style form elements (inputs, labels, buttons).
    - [ ] Style validation messages (error, success).
    - [ ] Ensure the modal is responsive.
- [ ] **Task 7: Accessibility Review & Implementation**
    - [ ] Verify `Dialog.Title` and `Dialog.Description` are correctly used.
    - [ ] Confirm `Form.Label` is correctly associated with form controls.
    - [ ] Test keyboard navigation (tabbing through elements, `Escape` key to close).
    - [ ] Test focus management (focus trapped in modal, returns to trigger on close).
    - [ ] Ensure error messages are announced by screen readers (Radix `Form.Message` should handle this).
    - [ ] Check color contrast for all elements.
- [ ] **Task 8: Backend Integration (Placeholder)**
    - [ ] Define the actual API endpoint for feedback submission.
    - [ ] Implement the `fetch` or `axios` call to send data to the backend.
    - [ ] Handle actual success and error responses from the backend, updating UI accordingly (replacing simulated logic from Task 4).

## Implementation Plan

### Key Architectural Decisions
*   The feedback modal will be a self-contained React component.
*   State management for the modal's visibility and form data will primarily use React's `useState` hook.
*   Radix UI components will be leveraged for core modal, form, and interactive element functionality to ensure accessibility and reduce boilerplate.

### Dependencies
*   `react`
*   `react-dom`
*   `typescript`
*   `@radix-ui/react-dialog` (for modal)
*   `@radix-ui/react-form` (for form structure and validation messages)
*   `@radix-ui/react-label` (for associating labels with inputs, if not using `Form.Label`)
*   `@radix-ui/react-text-area` (if preferred over `Form.Control as="textarea"`)
*   `@radix-ui/react-textfield` (if preferred over `Form.Control as="input"`)
*   `@radix-ui/react-slot` (potentially for component composition)
*   An icon library (e.g., `lucide-react` or `heroicons`) for the close 'X' icon, if desired.
*   Styling solution (e.g., CSS Modules, Tailwind CSS, Emotion, Styled Components - based on project setup).

### Coding Style
*   Functional components with React Hooks.
*   TypeScript for type safety.
*   Adherence to project's ESLint and Prettier configurations.
*   Clear separation of concerns where possible (e.g., UI, state logic, API calls).

### Backend Integration (Placeholder)
*   **API Endpoint:** `/api/feedback` (POST request)
*   **Request Payload (JSON):**
    ```json
    {
      "feedback": "string (required)",
      "name": "string (optional)",
      "email": "string (optional, valid email format if provided)"
    }
    ```
*   **Success Response (200 OK or 201 Created):**
    ```json
    {
      "message": "Feedback submitted successfully"
    }
    ```
*   **Error Responses:**
    *   `400 Bad Request`: For validation errors (e.g., empty feedback, invalid email if validated server-side).
        ```json
        {
          "error": "Invalid input",
          "details": { "feedback": "Feedback cannot be empty" }
        }
        ```
    *   `500 Internal Server Error`: For server-side issues.
        ```json
        {
          "error": "Failed to submit feedback due to a server error."
        }
        ```

## Relevant Files
*   `src/components/FeedbackModal/FeedbackModal.tsx` (Main component file)
*   `src/components/FeedbackModal/FeedbackModal.module.css` (Or other styling file depending on project setup, e.g., `FeedbackModal.styles.ts`)
*   `src/App.tsx` (Or relevant parent component where the `FeedbackModal` will be rendered/triggered) 