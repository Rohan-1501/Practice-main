# Documentation: StudentsController.cs

## Overview
`StudentsController.cs` is the primary hub for student-facing features, covering profile management and course enrollment.

## Detailed Explanation
The controller identifies the "Current Student" by looking at the `NameIdentifier` claim in their JWT token. This ensures that a student can only edit their *own* profile and see their *own* enrolled courses.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **11** | `[ApiController]` | **Attribute**: Enables automatic response status for model errors. |
| **12** | `[Route("api/students")]` | **Attribute**: Sets the base URL for student-specific services. |
| **15-16** | `private readonly ...` | **DI**: Private fields for Student and Enrollment registries. |
| **18** | `public StudentsController(...)` | **Constructor**: Receives repository dependencies via DI. |
| **25-26** | `[Authorize] [HttpGet("me")]` | **API**: Secure GET for the current student's personal profile. |
| **28-29** | `User.FindFirstValue(...)` | **Security**: Extracts the UserID from the cryptographically signed JWT. |
| **34** | `_students.GetByUserIdAsync(...)` | **Data**: Retrieves the detailed Student record from the database. |
| **38-51** | `s = new Student { ... }` | **Logic**: Auto-creates a profile if the user hasn't initialized one yet. |
| **60-61** | `[Authorize] [HttpPost("me")]` | **API**: High-performance "Upsert" endpoint for profile edits. |
| **71** | `_students.GetByUserIdTrackedAsync(...)` | **Logic**: Fetches with "Tracking" for easy database updates. |
| **90-104** | `if (req.FullName is not null) ...` | **Selective Update**: Only modifies fields present in the request. |
| **106** | `await _students.SaveChangesAsync(ct)` | **Persistence**: Flushes all changes to the SQL Server in one batch. |
| **112-128** | `private static ToMeDto(...)` | **Map**: Static conversion for transforming Entities into DTOs. |
| **132-133** | `[Authorize] [HttpPost("me/enroll")]` | **API**: Handles course registration for the current student. |
| **148** | `_enrollments.GetByStudentAndCourseAsync(...)` | **Guard**: Prevents duplication error by checking existing links. |
| **152-157** | `new Enrollment { ... }` | **Logic**: Creates a new linkage between the student and course. |
| **165-166** | `[Authorize] [HttpGet("me/courses")]` | **API**: Lists every course the student is currently enrolled in. |
| **178-192** | `enrollments.Select(...)` | **Logic**: Deep mapping of nested Course data into a linear list. |
| **199-200** | `[Authorize] [HttpDelete("me/courses/{id}")]` | **API**: Endpoint for unenrolling from a specific course. |
| **216** | `await _enrollments.RemoveAsync(enrollment)` | **Logic**: Deletes the persistent linkage from the database. |

## Program Flow
1. **Profile Recovery (`/me`)**: Tries to find the student profile. If missing (for a new account), it auto-creates a basic profile using details from the JWT claims.
2. **Upsert Profile**: Handles updating or initializing a student's personal/academic data. It uses a "selective update" approach, only changing fields that the user actually sent.
3. **Enrollment**: Links the current student to a specific course ID, while checking for duplicates.

## Connections
- **Depends on `IStudentRepository` & `IEnrollmentRepository`**.
- **Heavy use of `ClaimTypes.NameIdentifier`** to securely identify the user without requiring them to pass their own ID in the URL.

## Why it is used
It provides the "Self-Service" capability for students. It's the engine behind the "My Profile" and "Course Enrollment" pages.

## HTTP GET Methods: Detailed Explanation

### 1. `GetMe()`
- **Endpoint**: `GET /api/students/me`
- **Working**: Fetches the full profile of the currently authenticated student. If a profile doesn't exist yet (e.g., first-time login), it automatically initializes a basic one using the user's account identity claims.

### 2. `GetEnrolledCourses()`
- **Endpoint**: `GET /api/students/me/courses`
- **Working**: Queries the `EnrollmentRepository` to find every course the student is currently active in. It then returns a list of course details for the "My Learning" page.

## HTTP POST, PUT & DELETE Methods: Detailed Explanation

### 1. `UpsertMe()`
- **Endpoint**: `POST /api/students/me`
- **Working**: A robust "Profile Editor". It updates the student's personal details (DOB, Roll Number, Phone, etc.). It uses a "selective update" pattern, meaning if the frontend only sends a new Phone Number, the rest of the profile remains untouched.

### 2. `EnrollInCourse()`
- **Endpoint**: `POST /api/students/me/enroll`
- **Working**: The "Purchase/Enroll" action. It creates a connection between the student and a course in the `Enrollments` table. It also includes a guard to prevent a student from enrolling in the same course twice.

### 3. `UnenrollFromCourse(int courseId)`
- **Endpoint**: `DELETE /api/students/me/courses/{courseId}`
- **Working**: The "Leave Course" action. It locates the specific enrollment record for the current student and removes it from the database, instantly removing the course from their dashboard.

## Interview Questions
1. **Explain how `User.FindFirstValue(ClaimTypes.NameIdentifier)` works.**
   *Answer: It extracts the unique User ID embedded inside the JWT token. This is more secure than using an ID from the URL as the token is cryptographically signed and cannot be easily faked.*
2. **What is an "Upsert" and how is it implemented here?**
   *Answer: An Upsert is an operation that either Updates an existing record or Inserts a new one. In this controller, if `_students.GetByUserIdTrackedAsync` returns null, a new `Student` object is created and added to the database.*
3. **Why is the enrollment duplication check important?**
   *Answer: To prevent data corruption and unexpected UI behavior (like a student seeing the same course twice in their dashboard) and to maintain the integrity of analytics.*
