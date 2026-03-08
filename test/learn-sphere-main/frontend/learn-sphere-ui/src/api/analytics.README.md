# Documentation: analytics.js (Frontend)

## Overview
`analytics.js` is the data service responsible for fetching platform-wide business intelligence and performance metrics.

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1** | `import { http } from "./http";` | **Network**: Injects the base configuration for API requests. |
| **3-7** | `export const getSummaryStats = ...` | **API**: Fetches high-level counts (Students, Courses) for the Admin Dashboard. |
| **5** | `console.log(...)` | **Debug**: Outputs the raw payload for easier troubleshooting during development. |
| **9-13** | `export const getStudentPerformance = ...` | **API**: Retrieves per-student grade and completion data. |
| **15-19** | `export const getCoursePerformance = ...` | **API**: Fetches engagement and popularity metrics for specific courses. |

## Why it is used
To provide the data backbone for the Admin Dashboard. It transforms raw backend analytics into predictable frontend data streams.

## Interview Questions
1. **Why do you use `async/await` for these API calls?**
   *Answer: To handle the asynchronous nature of network requests in a way that looks like synchronous code, making it easier to read and maintain.*
2. **What is the significance of returning `response.data`?**
   *Answer: Axios returns a large response object containing headers, status codes, and the configuration. The UI only needs the actual JSON payload (the student/course data), which is stored in the `data` property.*
