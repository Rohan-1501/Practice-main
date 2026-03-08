# Documentation: ProtectedRoute.jsx

## Overview
`ProtectedRoute.jsx` is the primary firewall for the student dashboard.

## Detailed Explanation
It is a simple "Check" component. It looks for a `token` in the browser's `localStorage`. If the token is missing, it assumes the user is an intruder or has an expired session and redirects them immediately to the login page.

## Program Flow
1. **Look**: Accesses `localStorage.getItem("token")`.
2. **Redirect**: If null/undefined, returns `<Navigate to="/login" replace />`.
3. **Pass**: If the token exists, it renders the `children`.

## Why it is used
To protect student data. It ensures that pages like "My Courses" or "Profile" cannot be accessed by anyone who isn't authenticated.

## Interview Questions
1. **Is checking for a token in `localStorage` enough for "Real" security?**
   *Answer: No. A user could manually add a fake token to their storage. This component is only for "UI Security". Real security happens on the backend, where the server validates the signature of the token on every API request.*
2. **What happens if the token is present but expired?**
   *Answer: This specific component will still allow the user to see the page. However, as soon as the page tries to fetch data, the backend will return a `401 Unauthorized` error, and the application's global API interceptor will log the user out.*
