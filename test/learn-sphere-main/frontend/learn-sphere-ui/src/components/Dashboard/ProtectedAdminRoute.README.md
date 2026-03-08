# Documentation: ProtectedAdminRoute.jsx (Dashboard Version)

## Overview
`ProtectedAdminRoute.jsx` prevents students from accidentally (or intentionally) entering admin-only management areas.

## Detailed Explanation
This version of the admin guard is more sophisticated than the global one. It checks for two things:
1. **Authentication**: Is the user logged in at all? (Check for `token`).
2. **Authorization**: Is the user's role "admin"?

If the user is logged in as a student but tries to visit `/admin`, this component redirects them back to their own `/dashboard` instead of the login page.

## Program Flow
1. **Security Check**: Reads both `token` and `learnsphere_user` from storage.
2. **Redirect 1**: If no token, go to `/login`.
3. **Redirect 2**: If role is NOT admin, go to `/dashboard`.
4. **Grant**: If both pass, show the admin content.

## Why it is used
To ensure "Privilege Separation". It provides a better user experience for students who might follow an old link to an admin course editor.

## Interview Questions
1. **Explain the difference between "Authentication" and "Authorization".**
   *Answer: Authentication is "Who are you?" (handled by the token check). Authorization is "What are you allowed to do?" (handled by the role check). This component verifies BOTH.*
2. **Why does this component redirect non-admins to `/dashboard` instead of `/login`?**
   *Answer: To avoid confusing the user. If they are already logged in as a student, forcing them to log in again is frustrating. Sending them back to their home dashboard is more helpful.*
