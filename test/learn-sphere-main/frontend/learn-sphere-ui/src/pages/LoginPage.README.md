# Documentation: LoginPage.jsx

## Overview
`LoginPage.jsx` is the primary access point for authenticated users.

## Detailed Explanation
This page manages the "Handshake" between the user's credentials and the backend server. It is "Role-Aware"—meaning it checks whether the user is an admin or a student and sends them to the correct dashboard immediately after a successful login.

## Program Flow
1. **Validation**: Normalizes the email to lowercase before sending it.
2. **Authentication**: Calls `loginApi` and awaits the JWT.
3. **Storage**:
   - Saves the JWT as `token`.
   - Saves a serialized `learnsphere_user` object (Role, Name, Email).
4. **Notification**: Dispatches `userUpdated` so the Navbar can update its UI.
5. **Branching**:
   - If Role == "admin", navigate to `/admin`.
   - Otherwise, navigate to `/dashboard`.

## Why it is used
To secure the application. It acts as the "Bouncer" that verifies identities and sets the global state for the rest of the application session.

## Interview Questions
1. **How do you handle the discrepancy between C# (PascalCase) and JS (camelCase) naming conventions in the login response?**
   *Answer: By using "Fallback Checks". When reading the API response, we check for both `data.token` AND `data.Token`. This makes the frontend resilient to changes in backend naming conventions.*
2. **Why do we dispatch a `CustomEvent` after login?**
   *Answer: Decoupling. The `LoginPage` doesn't know about the `Navbar`. Instead of passing callbacks through props (which is messy), we broadcast a signal to the whole window. The `Navbar` is "listening" for this signal and refreshes itself automatically.*
3. **Explain the `noValidate` attribute on the form.**
   *Answer: Custom UX. By disabling the browser's default validation, we can use our own `InputField` component and custom error messages, which allows for a more beautiful and consistent design.*
