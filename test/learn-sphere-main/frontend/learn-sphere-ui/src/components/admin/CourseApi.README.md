# Documentation: CourseApi.jsx (Admin Wrapper)

## Overview
`CourseApi.jsx` is a helper service that simplifies communication between the Admin UI and the Course API.

## Detailed Explanation
This file centralizes all logic for creating, updating, and deleting courses. It handles the "Translation" between what the React components provide (like an array of categories) and what the C# backend expects (a comma-separated string). It also features built-in error handling for common HTTP statuses like `403 Forbidden` and `404 Not Found`.

## Program Flow
1. **Request**: A React component calls `createCourse(data)`.
2. **Formatting**: The API wrapper trims strings, converts numbers, and flattens arrays.
3. **Execution**: Sends a request using the `http` axios instance.
4. **Error Mapping**: If the server fails, it throws a human-readable error message (e.g., "Only admins can create courses") instead of a generic "Network Error".

## Why it is used
To keep the UI components "Clean". By putting the logic for formatting and error handling here, the `CourseForm` component can stay focused on the user interface, not API quirks.

## Interview Questions
1. **Why do we convert the `categories` array into a string before sending it?**
   *Answer: Backend constraints. The SQL database stores categories as a simple string column. This utility handles that conversion so the frontend can continue working with a modern array structure.*
2. **Explain the purpose of `getCourseBySlug`.**
   *Answer: It allows for "Offline Filtering". Instead of calling a specific backend endpoint for a slug, it fetches all courses and finds the match locally. This is useful for building high-performance search and preview features.*
3. **What is the benefit of the `403` and `401` error checks in this file?**
   *Answer: It provides clear feedback to the developer and user. Instead of a silent failure, the code explicitly tells you if your session has expired or if you lack the necessary permissions.*
