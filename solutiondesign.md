# Solution Design: Feedback Modal

## 1. Introduction

This document outlines the solution design for a user feedback modal. The modal allows users to submit feedback, optionally providing their name and email address. The feature is triggered by a button located at the bottom right of the screen.

**Tech Stack:** React, TypeScript, Vite, Radix UI

## 2. Actors

*   **User:** Any individual interacting with the application who wishes to provide feedback.

## 3. UI Components (Radix UI Considerations)

*   **Trigger Button:** A standard HTML button or Radix UI `Button` component, styled and positioned at the bottom right with the label "Ihr Feedback".
*   **Modal Dialog:** Radix UI `Dialog` (composed of `Dialog.Root`, `Dialog.Trigger`, `Dialog.Portal`, `Dialog.Overlay`, `Dialog.Content`, `Dialog.Title`, `Dialog.Description`, `Dialog.Close`).
*   **Text Input for Feedback:** Radix UI `TextArea` or a styled `<textarea>`.
*   **Text Input for Name:** Radix UI `TextField.Root` and `TextField.Input` or a styled `<input type="text">`.
*   **Text Input for Email:** Radix UI `TextField.Root` and `TextField.Input` or a styled `<input type="email">`.
*   **Submit Button:** Radix UI `Button` within the modal (e.g., labeled "Senden").
*   **Close Button:** Radix UI `Dialog.Close` (can be an 'X' icon or a "Schließen" button).
*   **Form Handling:** Radix UI `Form` (composed of `Form.Root`, `Form.Field`, `Form.Label`, `Form.Control`, `Form.Message`, `Form.ValidityState`, `Form.Submit`) can be used to manage form state, validation, and submission.

## 4. Use Cases & User Flows

### UC-001: User Submits Feedback with All Optional Information

*   **Goal:** User successfully submits feedback along with their name and email.
*   **Preconditions:**
    *   The application is loaded.
    *   The "Ihr Feedback" button is visible in the bottom right corner.
*   **User Flow:**
    1.  **User** clicks the "Ihr Feedback" button.
    2.  **System** displays the feedback modal. The modal includes:
        *   A title (e.g., "Geben Sie uns Ihr Feedback").
        *   A multi-line text area for the feedback message (e.g., placeholder "Ihr Feedback..."). This field is mandatory.
        *   An optional text input field for "Name" (e.g., placeholder "Ihr Name (optional)").
        *   An optional text input field for "E-Mail" (e.g., placeholder "Ihre E-Mail-Adresse (optional)").
        *   A "Senden" (Submit) button.
        *   A "Schließen" (Close) button or an 'X' icon.
    3.  **User** types their feedback into the feedback text area.
    4.  **User** types their name into the "Name" input field.
    5.  **User** types their email address into the "E-Mail" input field.
    6.  **(Optional Client-Side Validation Step) System** validates the email format if an email is provided. If invalid, an appropriate message is shown near the email field, and the "Senden" button might be disabled or submission prevented until corrected.
    7.  **User** clicks the "Senden" button.
    8.  **System** validates that the feedback text area is not empty. (If it is, show an error and prevent submission - see UC-004).
    9.  **System** processes the feedback submission (e.g., sends it to a backend API).
        *   Displays a loading indicator on the "Senden" button or within the modal.
        *   Disables the "Senden" and input fields during submission.
    10. **System (Success):**
        *   Hides the loading indicator.
        *   Enables form fields if they were disabled (though they will be cleared).
        *   Displays a success message within the modal (e.g., "Vielen Dank für Ihr Feedback!") or closes the modal and shows a global success notification (toast).
        *   Clears all form fields (feedback, name, email).
        *   Optionally, automatically closes the modal after a short delay if the success message is shown within it and no further action is required.
*   **Postconditions:**
    *   Feedback is successfully submitted with name and email.
    *   Modal is closed or displays a success state with cleared fields.

### UC-002: User Submits Feedback Without Optional Information

*   **Goal:** User successfully submits feedback without providing their name or email.
*   **Preconditions:**
    *   The application is loaded.
    *   The "Ihr Feedback" button is visible.
*   **User Flow:**
    1.  **User** clicks the "Ihr Feedback" button.
    2.  **System** displays the feedback modal (as in UC-001).
    3.  **User** types their feedback into the feedback text area.
    4.  **User** leaves the "Name" and "E-Mail" input fields empty.
    5.  **User** clicks the "Senden" button.
    6.  **System** validates that the feedback text area is not empty.
    7.  **System** processes the feedback submission.
        *   Displays a loading indicator and disables fields.
    8.  **System (Success):**
        *   Hides the loading indicator and enables fields.
        *   Displays a success message.
        *   Clears the feedback form field.
        *   Optionally, closes the modal.
*   **Postconditions:**
    *   Feedback is successfully submitted without name and email.
    *   Modal is closed or displays a success state with the feedback field cleared.

### UC-003: User Opens and Closes Modal Without Submitting

*   **Goal:** User decides not to submit feedback and closes the modal.
*   **Preconditions:**
    *   The application is loaded.
    *   The "Ihr Feedback" button is visible.
*   **User Flow:**
    1.  **User** clicks the "Ihr Feedback" button.
    2.  **System** displays the feedback modal.
    3.  **User** may or may not interact with the input fields.
    4.  **User** clicks the "Schließen" button, the 'X' icon, or presses the `Escape` key.
    5.  **System** closes the feedback modal.
    6.  **System** ensures any data entered into the form fields is cleared so it's not present the next time the modal is opened.
*   **Postconditions:**
    *   Modal is closed.
    *   No feedback is submitted.
    *   Form fields are reset.

### UC-004: User Attempts to Submit Empty Feedback

*   **Goal:** System prevents submission of empty feedback and informs the user.
*   **Preconditions:**
    *   Feedback modal is open.
*   **User Flow:**
    1.  **User** has opened the feedback modal.
    2.  **User** does not type any message in the feedback text area (it remains empty or contains only whitespace).
    3.  **User** clicks the "Senden" button.
    4.  **System** (client-side validation) validates that the feedback text area is empty or contains only whitespace.
    5.  **System** displays an error message next to the feedback text area or a general modal error message (e.g., "Bitte geben Sie Ihr Feedback ein."). The field might be highlighted.
    6.  **System** does not submit the form. The modal remains open, and focus might be set to the feedback text area.
    7.  **User** can then either enter feedback (UC-001, UC-002) or close the modal (UC-003).
*   **Postconditions:**
    *   No feedback is submitted.
    *   Modal remains open with an error message displayed, indicating the feedback field is required.

### UC-005: User Submits Feedback with Invalid Email Format

*   **Goal:** System prevents submission if an email is provided but is in an invalid format, while still allowing submission if the email field is cleared.
*   **Preconditions:**
    *   Feedback modal is open.
    *   User has entered text in the feedback text area.
    *   User has entered text in the email field.
*   **User Flow:**
    1.  **User** has opened the feedback modal and filled the feedback text area.
    2.  **User** types an invalid email (e.g., "test@", "test.com", "invalid email") into the "E-Mail" input field.
    3.  **User** clicks the "Senden" button.
    4.  **System** (client-side validation) validates the email format if the field is not empty.
    5.  **System** detects the invalid email format.
    6.  **System** displays an error message next to the "E-Mail" input field (e.g., "Bitte geben Sie eine gültige E-Mail-Adresse ein oder lassen Sie das Feld leer."). The field might be highlighted.
    7.  **System** does not submit the form. The modal remains open.
    8.  **User** can then:
        *   Correct the email address and attempt submission again.
        *   Clear the email address field (making it optional again) and attempt submission.
        *   Close the modal (UC-003).
*   **Postconditions:**
    *   No feedback is submitted if the email format is invalid and the field is not empty.
    *   Modal remains open with an error message regarding the email format.

### UC-006: System Handles Submission Error (e.g., Network Error, Server Error)

*   **Goal:** System informs the user about an unsuccessful submission attempt and preserves their input.
*   **Preconditions:**
    *   Feedback modal is open.
    *   User has filled the feedback text area (and optionally other fields).
*   **User Flow:**
    1.  **User** clicks the "Senden" button after filling in the feedback correctly (passes client-side validation).
    2.  **System** attempts to process the feedback submission (sends data to the backend).
        *   Displays a loading indicator and disables fields.
    3.  **System (Error):** A network error occurs, or the backend API returns an error (e.g., 5xx status code).
    4.  **System:**
        *   Hides the loading indicator.
        *   Enables form fields.
        *   Displays an error message within the modal (e.g., "Ihr Feedback konnte nicht gesendet werden. Bitte versuchen Sie es später erneut.").
        *   The entered data in the form fields (feedback, name, email) MUST remain, allowing the user to retry without re-typing everything.
    5.  **User** can then attempt to click "Senden" again or close the modal.
*   **Postconditions:**
    *   No feedback is submitted to the backend.
    *   Modal remains open with an error message.
    *   User's input is preserved for a retry.

## 5. Accessibility Considerations (using Radix UI)

*   **Labels:** Ensure all form elements (`TextArea`, `TextField.Input`) are associated with `Form.Label`.
*   **Modal Semantics:** `Dialog.Title` and `Dialog.Description` should be used to provide context to assistive technologies.
*   **Focus Management:** Radix UI `Dialog` typically handles trapping focus within the modal and returning focus to the trigger element when closed. Verify this behavior.
*   **Keyboard Navigation:** Ensure all interactive elements (buttons, inputs) are keyboard accessible and the modal can be closed using the `Escape` key (default Radix UI `Dialog` behavior).
*   **Error Feedback:** Errors should be clearly communicated, associated with the respective fields (using `Form.Message`), and accessible to screen readers (e.g., via `aria-live` regions for dynamic messages if not handled by Radix `Form.Message`).
*   **Visual Feedback:** Provide clear visual cues for states like loading (e.g., spinner on button), success (e.g., message, checkmark), and error (e.g., message, field highlighting).
*   **Color Contrast:** Ensure sufficient color contrast for text, UI elements, and state indicators as per WCAG guidelines.

## 6. Future Enhancements (Optional)

*   Allow users to select a category for their feedback (e.g., "Bug Report", "Feature Request", "General Feedback") using Radix UI `Select` or `RadioGroup`.
*   Allow file attachments (e.g., screenshots) - this would require backend support and more complex UI.
*   Implement a character counter/limit for the feedback text area.
*   Client-side debouncing/throttling for input validation to avoid excessive error messages while typing.
*   More sophisticated global notification system (toast) for success/error messages upon modal close, using Radix UI `Toast`.
*   "Rate your experience" (e.g., star rating) alongside the text feedback. 