# Documentation: enrollmentApi.js

## Overview
`enrollmentApi.js` manages the "Contract" between a student and a course.

## Detailed Explanation
This is a lightweight service focused on the three core enrollment actions:
- **Enroll**: Joining a new course.
- **Fetch**: Getting a list of everything the student is currently studying.
- **Unenroll**: Leaving a course and removing it from the dashboard.

## Program Flow
- **Static Endpoint**: It uses the `students/me` base URL, which is a common REST pattern where the server identifies "Who is calling" based on their token, rather than needing an ID in the URL.
- **Validation**: It specifically catches a `400 Bad Request` and translates it into "Already enrolled in this course", which is more helpful to the user.

## Why it is used
To provide a dedicated space for enrollment logic. This ensures that the enrollment process is isolated from the content management process, making the code more modular.

## Interview Questions
1. **Why does `getEnrolledCoursesAPI` return an empty array `[]` on error?**
   *Answer: Defensive Programming. If the API fails (e.g., server is down), we don't want the UI to "Crash" because it's trying to loop through `null`. By returning `[]`, the UI will simply show "No courses found", which is a "Graceful Degradation" of the experience.*
2. **What is the advantage of using a `students/me` endpoint?**
   *Answer: Security. By using `/me`, we don't allow a student to pass an arbitrary ID (like `/student/500`) to try and enroll someone else. The server looks at the JWT token, finds the real student ID, and performs the action only for them.*
  
