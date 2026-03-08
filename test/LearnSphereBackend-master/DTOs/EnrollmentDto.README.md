# Documentation: EnrollmentDto.cs

## Overview
`EnrollmentDto.cs` handles simple request and response objects for joining a course.

## Detailed Explanation
- `EnrollmentRequestDto`: Used when a student clicks "Enroll". It only needs the `CourseId` because the `StudentId` is identified from the user's login session.
- `EnrollmentResponseDto`: The data sent back after enrollment is successful, including the new `Id` and the `EnrolledAt` date.

## Why it is used
It simplifies the enrollment process by only requiring the minimum amount of data from the frontend.

## Interview Questions
1. **Why doesn't `EnrollmentRequestDto` include a `StudentId`?**
   *Answer: For security. The server should always identify the student from their authenticated JWT token rather than trusting a `StudentId` sent in the request body, which could be tampered with.*
2. **What does the "active" status represent in `EnrollmentResponseDto`?**
   *Answer: It is the default state of a new enrollment, meaning the student has access to the course but hasn't completed it yet.*
