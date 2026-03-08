# Documentation: API Auxiliary Services

## Overview
This documentation covers the smaller, specialized API services: `authApi.js`, `analytics.js`, `notificationApi.js`, `studentApi.js`, and `userApi.js`.

## Detailed Explanation
These files are thin "Wrappers" around the Axios `http` instance. They exist to give semantic names to backend endpoints:
- **`authApi.js`**: Pure entry/exit (Login and Register).
- **`studentApi.js`**: Profile management for the logged-in student (`/me`).
- **`userApi.js`**: Administrative tools for managing all users (Admin only).
- **`analytics.js`**: Data aggregation for reports.
- **`notificationApi.js`**: Real-time unread count and polling logic.

## Why they are used
To maintain the "Service Layer" pattern. By separating these into their own files, we ensure that the source code for a specific feature (like Profile Editing) is easy to find and doesn't conflict with unrelated code (like Admin User Management).

## Interview Questions
1. **Why use specialized files like `studentApi.js` vs. one giant `api.js`?**
   *Answer: Maintainability and Bundle Size. In a massive app, one file with 500 exports is a nightmare to navigate. Breaking them by domain (Student, User, Course) makes the code much cleaner and allows for easier tree-shaking during the build process.*
2. **Explain the `take` parameter in `notificationApi.list`.**
   *Answer: Pagination Control. We don't want to fetch 1,000 notifications at once. The `take` parameter tells the server "Only give me the most recent 20". This saves bandwidth and makes the app load faster.*
3. **What is the purpose of the `console.log` in `analytics.js`?**
   *Answer: Debugging Visibility. Since analytics often involve complex data structures (numbers, arrays, nested stats), having the data automatically logged in development helps developers verify that the UI calculations match the raw server data.*
  
