# Documentation: userApi.js (Frontend)

## Overview
`userApi.js` provides the administrative interface for managing the platform's user base.

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1** | `import { http } from "./http";` | **Network**: Imports the shared communication layer. |
| **5-13** | `getAll: async () => { ... }` | **API**: Secure endpoint to fetch the full list of platform users. |
| **16-24** | `updateStatus: async (...) => { ... }` | **API**: Allows admins to activate or deactivate user accounts remotely. |
| **18** | `http.put(..., { status })` | **Mutation**: Sends a PUT request with a partial state update for the user. |

## Why it is used
To centralize user management networking. It ensures that role-based status changes are handled consistently across the admin interface.

## Interview Questions
1. **Explain the use of `try/catch` in these API methods.**
   *Answer: Error Resilience. If the server is down or the user is unauthorized (403), the `catch` block logs the specific error and re-throws it so the UI can display a helpful notification to the admin.*
2. **When would you use a `PUT` request versus a `PATCH` request in this file?**
   *Answer: In this implementation, `PUT` is used even for partial status updates. However, `PATCH` would be more semantically correct if we were only changing the `status` field without sending the whole user object.*
