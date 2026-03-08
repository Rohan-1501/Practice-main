# Documentation: EnrollmentRepository.cs

## Overview
`EnrollmentRepository.cs` manages the relationship between students and courses.

## Detailed Explanation
It provides helper methods to find a specific enrollment (to prevent duplicates) and to fetch a student's entire course list. It heavily uses `.Include(e => e.Course)` so that the frontend can display course titles and thumbnails within the student's dashboard.

## Line-by-Line Explanation: IEnrollmentRepository.cs

| Line(s) | Explanation |
| :--- | :--- |
| **5** | Defines the interface for managing student-course linkages. |
| **7** | `GetByStudentAndCourseAsync`: Contract to find a specific entry (useful for duplicate checks). |
| **8-9** | `GetStudentEnrollmentsAsync` / `GetStudentCoursesAsync`: Contracts to list what a student is learning. |

## Line-by-Line Explanation: EnrollmentRepository.cs

| Line(s) | Explanation |
| :--- | :--- |
| **1-4** | Imports for Context, Models, and Interface contract. |
| **8** | Class declaration implementing the `IEnrollmentRepository`. |
| **12-15** | **Constructor**: Receives the `AppDbContext` via for DB access. |
| **17-21** | `GetByStudentAndCourseAsync`: Efficiently finds a unique link between a student and a course. |
| **23-29** | `GetStudentEnrollmentsAsync`: Fetches all enrollments for a user and **eager-loads** the `Course` details. |
| **31-38** | `GetStudentCoursesAsync`: A specialized query that returns the `Course` objects directly using `.Select()`. |
| **40-45** | `AddAsync`: Persists a new enrollment record and returns it. |
| **47-51** | `RemoveAsync`: Deletes an enrollment (unenrollment) and saves the change. |
| **53-56** | `SaveAsync`: Manual trigger to commit all pending context changes to the database. |

## Program Flow
1. **Link**: Creates a new `Enrollment` record when a student joins.
2. **Retrieve**: Fetches all courses a specific student is currently "Active" in.

## Why it is used
It acts as the bridge for the "Student Journey". Most of the student dashboard logic relies on finding data through this repository.

## Interview Questions
1. **How do you prevent a student from enrolling twice in the same course?**
   *Answer: By using `GetByStudentAndCourseAsync` to check if a record already exists before calling `AddAsync` in the controller.*
2. **What is the purpose of the `GetStudentCoursesAsync` method?**
   *Answer: It is a simplified query that uses `.Select(e => e.Course!)` to return a list of `Course` objects directly, rather than returning `Enrollment` objects that contain courses.*
3. **Why do we need a separate `SaveAsync` method here?**
   *Answer: It allows the developer to make multiple changes to the database (e.g., adding an enrollment and updating a counter) and then save them all as a single "Transaction" for better data integrity.*
