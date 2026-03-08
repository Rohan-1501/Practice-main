# Documentation: courseApi.js

## Overview
`courseApi.js` is the most important service in the application, managing the entire lifecycle of learning content.

## Detailed Explanation
This file translates complex user actions into structured backend requests. It is divided into four main sections:
1. **Courses**: Basic CRUD (Create, Read, Update, Delete) and structure fetching.
2. **Chapters & Lessons**: Hierarchical management of where content sits.
3. **Quizzes & Assessments**: Logic for submission, scoring, and eligibility checks.
4. **Live Sessions**: Scheduling and attendance tracking.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Imports the base `http` instance from the local config. |
| **3-5** | **Path Constants**: Defines the primary resource routes for the backend API. |
| **9-17** | `getAll`: Fetches the basic list of all courses for the student catalog. |
| **20-28** | `getById`: Retrieves the basic metadata for a specific course. |
| **31-51** | `create`: Validates inputs and handles admin-specific error mapping (400, 401, 403). |
| **54-64** | `update`: Standard admin-only PUT request for changing course details. |
| **80-88** | `getStructure`: Fetches the heavy nested JSON (chapters/lessons) for the course tree. |
| **91-103** | `getBySlug`: Client-side search that finds a course by its URL-friendly slug. |
| **118-155** | **Chapters**: Sub-object containing CRUD logic for course sections. |
| **158-191** | **Lessons**: Sub-object for managing individual lesson content within chapters. |
| **194-226** | **Content**: Multipart upload logic for handling video and PDF files. |
| **233-261** | `quizApi`: Independent service for managing MCQ tests and submissions. |
| **267-317** | `assessmentApi`: Logic for high-stakes exams, eligibility, and lesson completion tracking. |
| **322-346** | `liveSessionApi`: Manages real-time instructor sessions and "Join" actions. |

## Program Flow
- **Abstraction**: Instead of components writing `http.post('courses/5/chapters/2/lessons/assessment')`, they simply call `courseApi.completeLesson(lessonId)`.
- **Slug Support**: It includes "Helper Methods" like `getBySlug` which first fetch all courses and then find the match locally, providing a smoother experience for the user.
- **Error Mapping**: When a request fails, this API "Re-throws" the error with a human-readable message (e.g., "You must be an admin to create courses").

## Why it is used
To keep the UI code clean. By moving all URLs and data-mapping logic into this file, the React components (like `CoursePlayerPage`) can focus purely on the visual experience rather than the details of the API.

## Interview Questions
1. **Explain the benefits of nesting the standard API calls (e.g., `courseApi.chapters.create`).**
   *Answer: Discoverability and Namespace. It reflects the "Tree" structure of the data. When a developer types `courseApi.content.`, their editor will automatically suggest `upload` and `delete`. This makes the codebase much easier to learn and maintain.*
2. **How does `completeLesson` differ from a regular update?**
   *Answer: Semantic Action. `completeLesson` is a specialized endpoint that doesn't just change a value; it might trigger other events on the server, like checking if the student is now eligible for a certificate or calculating their progress percentage.*
3. **Why do many of these methods use `async/await`?**
   *Answer: Readability. API calls are "Asynchronous" (taking time to complete). `async/await` allows us to write code that looks sequential and is easy to debug, even though it's actually waiting for a response from a server thousands of miles away.*
  
