# Documentation: RegistrationForm.jsx

## Overview
`RegistrationForm.jsx` is the gateway for new students to join the LearnSphere platform.

## Detailed Explanation
This component manages a multi-field state (Name, Email, Password, Confirm Password). It features "Real-time Validation"—as the user types in their email, the form instantly checks if that email is already taken by calling a mock/real API. It also ensures that passwords match and meet the platform's high-security complexity standards.

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **13-18** | `const [form, setForm] = ...` | **State**: Holds the reactive data for the 4-field registration packet. |
| **24-55** | `validateField = async (...)` | **Logic**: Per-field validation engine that checks for emptiness, format, and confirm-matches. |
| **36** | `await checkDuplicateEmail(...)` | **Async**: Real-time uniqueness check that queries the backend as the user types. |
| **57-61** | `onChange = async (e) => ...` | **Event**: Dual-purpose handler that updates state and triggers validation in parallel. |
| **63-99** | `validateAll = async () => ...` | **Logic**: Final "Big Check" that runs before submission to ensure data integrity. |
| **114-118** | `await registerApi({ ... })` | **Async**: Executes the backend user creation orchestrator. |
| **123-131** | `localStorage.setItem(...)` | **Security**: Persists the new JWT and profile so the user stays logged in after signing up. |
| **137** | `dispatchEvent(new Event(...))` | **Sync**: Global event trigger to refresh the Navbar state across the SPA. |
| **138** | `navigate("/dashboard")` | **Navigation**: Redirects the new student to their personalized learning hub. |
| **153-190** | `<InputField ... />` | **UI**: High-end form inputs with delegated state and error display management. |

## Why it is used
To provide a secure and frictionless onboarding experience. By catching errors (like a weak password or a duplicate email) BEFORE the user clicks submit, it reduces frustration and server load.

## Interview Questions
1. **How do you handle "Asynchronous Validation" for the email field?**
   *Answer: Inside the `validateField` function, if the field is "email" and it's syntactically valid, we await a call to `checkDuplicateEmail(value)`. This ensures we tell the user the email is taken while they are still on that input field, rather than waiting for them to finish the whole form.*
2. **Explain the purpose of `window.dispatchEvent(new Event("userUpdated"))`.**
   *Answer: This is a "Global Signal". The Navbar needs to know that a user has just logged in so it can change "Login/Register" buttons into a "Profile" icon. Since the RegistrationForm and Navbar aren't directly related, the custom event is the cleanest way to notify the Navbar.*
3. **Why do we "Normalize" the email before sending it to the API?**
   *Answer: To prevent accidental duplicate accounts. A user might type `John@Gmail.com` while another types `john@gmail.com`. Normalization ensures they are treated as the same identity on the backend.*
