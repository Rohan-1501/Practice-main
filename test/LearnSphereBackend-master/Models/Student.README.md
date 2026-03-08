# Documentation: Student.cs

## Overview
`Student.cs` represents the detailed profile of a student. Unlike the `User` model which handles auth, this model handles personal, academic, and guardian information.

## Detailed Explanation
This model is designed to collect comprehensive data about a student, from their date of birth to their academic year and parent/guardian contact info. It also serves as the container for all of a student's `Enrollments`.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Defines the namespace for the student entity. |
| **3** | Core `Student` class definition for extended profiles. |
| **5** | `Id`: Primary key for the student record. |
| **7-8** | **Foreign Key**: Connects the profile back to the `User` (identity) record. |
| **11-13** | **Personal**: Stores Name, Birthday (`DateOnly` for safety), and Gender. |
| **14** | `Email`: Primary contact email (usually mirrors the User email). |
| **15-16** | **Contact**: Geographical and telephone metadata for demographics. |
| **19-21** | **Academic**: Track identifiers like Roll Number, current course, and year level. |
| **24-27** | **Guardian**: Emergency contact details for the student's legal sponsors. |
| **30** | **Navigation**: Collection of course `Enrollments` established via the enrollment join table. |

## Program Flow
1. **Profile Creation**: When a user marks themselves as a student, a record in the `Students` table is created and linked to their `UserId`.
2. **Dashboard Display**: When a student logs in, the UI fetches this profile to display their name, course, and enrolled classes.
3. **Progress Tracking**: The `Enrollments` collection is used to calculate course completion percentages.

## Connections
- **Connected to `User`**: Linked via `UserId` (Foreign Key).
- **Connected to `Enrollment`**: One student can have many enrollments.
- **Used by `StudentsController`**: To manage student profiles and personal info updates.

## Properties Explanation
- `UserId`: Links the profile to the authentication record.
- `FullName`: The student's name used for academic records.
- `RollNumber`: Unique identifier within an institution.
- `Course`: The specific degree or program the student is enrolled in (e.g., "Computer Science").
- `GuardianInfo`: Fields like `GuardianName` and `GuardianPhone` for administrative contact.

## Why it is used
It encapsulates all domain-specific data for a student. Keeping this separate from the `User` model conforms to the Single Responsibility Principle (SRP).

## Interview Questions
1. **How does the `UserId` property function here?**
   *Answer: It acts as a Foreign Key that links back to the `User` model, establishing a 1:1 relationship between a login account and a student profile.*
2. **When would you use `DateOnly?` instead of `DateTime?` for `DateOfBirth`?**
   *Answer: `DateOnly` is preferred for birthdays because the time of day is irrelevant and can cause timezone-related bugs if stored as `DateTime`.*
3. **What is the benefit of having `GuardianInfo` directly in the Student model instead of a separate table?**
   *Answer: For a simple system, it reduces join complexity. However, if multiple students shared the same guardian, a separate table would be better for normalization.*
