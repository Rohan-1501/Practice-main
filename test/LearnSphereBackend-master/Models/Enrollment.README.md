# Documentation: Enrollment.cs

## Overview
`Enrollment.cs` is a "Join Model" that connects a `Student` to a `Course`. It tracks the relationship, progress, and performance of a student within a specific course.

## Detailed Explanation
This model goes beyond a simple link; it stores auditing and analytics data like the date of enrollment, the student's current grade, score, and even attendance logs.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **3** | Core `Enrollment` class acting as the student-course bridge. |
| **5** | `Id`: Unique `Guid` pk for the enrollment entry. |
| **7-8** | **Student Link**: Foreign key for the student who enrolled. |
| **10-11** | **Course Link**: Foreign key for the class joined. |
| **13** | `EnrolledAt`: The date the student officially joined the course. |
| **14** | `CompletedAt`: The date student finished all course criteria. |
| **15** | `Status`: Enum-like string (active, completed, dropped). |
| **18** | `Grade`: The letter grade awarded (A, B, C, etc.). |
| **19** | `Score`: Numerical grade (e.g., 95.5) for fine-grained ranking. |
| **20** | `Compliance`: HR/Corporate field for tracking mandatory training status. |
| **21** | `Attendance`: Text blob storing specific dates of participation. |
| **22** | `AttendanceCount`: Cached integer for quick reporting on frequency. |

## Program Flow
1. **Course Signup**: When a student clicks "Enroll", a new `Enrollment` record is created.
2. **Progress Updates**: As the student completes lessons, analytics fields like `Score` or `AttendanceCount` are updated.
3. **Completion**: When all requirements are met, the `Status` changes to "completed" and `CompletedAt` is timestamped.

## Connections
- **Connected to `Student`**: Many Enrollments belong to one Student.
- **Connected to `Course`**: Many Enrollments link to one Course.
- **Used by `EnrollmentRepository`**: To query which students are in which courses.

## Properties Explanation
- `StudentId` & `CourseId`: The foreign keys that establish the relationship.
- `EnrolledAt`: Records when the student joined.
- `Status`: tracks if the student is "active", has "completed" the course, or has "dropped".
- `Score`: A numerical representation of the student's performance (e.g., average quiz score).
- `AttendanceCount`: number of live sessions or lessons attended.

## Why it is used
It resolves the Many-to-Many relationship between Students and Courses. It also provides a centralized place to store metrics relevant to THAT specific student in THAT specific course.

## Interview Questions
1. **Why is this called a "Join Table" or "Bridge Entity"?**
   *Answer: Because it facilitates a Many-to-Many relationship between Students and Courses, which relational databases cannot handle directly.*
2. **What is the importance of the `Status` field in business logic?**
   *Answer: It allows for soft-management of course access. For example, a "dropped" status keeps the data for history but denies the student access to course materials.*
3. **If you wanted to see the total number of students in a course, which table would you query?**
   *Answer: You would count the `Enrollment` records filtering by the specific `CourseId`.*
