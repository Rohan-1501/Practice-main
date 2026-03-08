# Documentation: ProtectedAdminRoute.jsx

## Overview
`ProtectedAdminRoute.jsx` is a security wrapper that blocks non-admin users from seeing restricted pages.

## Detailed Explanation
It is a simple "Gatekeeper" component. It sits around admin pages in the routing configuration. If it detects that the logged-in user isn't an admin (or isn't logged in at all), it forcibly redirects them back to the Login page using React Router's `<Navigate />`.

## Program Flow
1. **Check**: Tries to parse the `learnsphere_user` from `localStorage`.
2. **Validation**: Checks if `user.role === "admin"`.
3. **Decision**:
   - If Admin: Renders the `children` (the actual page).
   - If Not: Redirects to `/login` with the `replace` flag (so they can't click "Back" to get into the admin panel).

## Why it is used
Front-end security is essential for user experience. While the backend ALSO checks roles, this component prevents a student from even seeing the layout of the admin panel.

## Interview Questions
1. **Why is the `JSON.parse` inside a `try-catch` block?**
   *Answer: Robustness. If the `localStorage` string is corrupted or empty, `JSON.parse` will crash the whole application. The `try-catch` ensures that any error simply results in a safe redirect to the login page.*
2. **What does the `replace` prop on the `<Navigate />` component do?**
   *Answer: It replaces the current entry in the browser history. This prevents the "Back Button Loop" where a user keeps getting redirected back to the login page when they click their browser's back button.*
