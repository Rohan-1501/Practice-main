# Documentation: LessonProgress.cs

## Overview
`LessonProgress.cs` is a tracking model that records whether a specific student has finished a specific lesson.

## Detailed Explanation
This is a standard "junction" model with extra metadata (`IsCompleted`, `CompletedAt`). It allows the system to remember where a student left off and show progress bars on the frontend.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for standard property attributes. |
| **6** | Defines the `LessonProgress` class for tracking student activity. |
| **9** | `Id`: Primary key for the progress record. |
| **12-15** | **Student Link**: Foreign key connecting the progress to a specific profile. |
| **18-21** | **Lesson Link**: Foreign key identifying which learning unit this refers to. |
| **23** | `IsCompleted`: Boolean flag (bit in SQL) toggled when finished. |
| **25** | `CompletedAt`: The exact timestamp when the completion event occurred. |

## Program Flow
1. **Starting a Lesson**: When a student opens a lesson for the first time, the system checks for a progress record.
2. **Marking Complete**: When the student finishes the video or clicks "Mark as Done", the `IsCompleted` flag is set to true and a timestamp is recorded.
3. **Progress Calculation**: The dashboard aggregates these records to show "X% Complete" for the whole course.

## Connections
- **Connected to `Student`**: Tracks progress for a specific student.
- **Connected to `Lesson`**: Links the progress to a specific lesson.

## Properties Explanation
- `StudentId` & `LessonId`: Foreign keys identifying the unique student-lesson combination.
- `IsCompleted`: Boolean flag indicating if the lesson is done.
- `CompletedAt`: The exact time the student finished the lesson.

## Why it is used
It enables the "Learning State" of the application. Without it, students would have to manually remember what they've already watched/read every time they log in.

## Interview Questions
1. **Why not just add an `IsCompleted` list directly to the Student model?**
   *Answer: Because that would violate normalization. A separate model allows for easier querying (e.g., "How many people finished Lesson X today?") and keeps the Student model lean.*
2. **What happens if a student watches a lesson twice?**
   *Answer: The system usually checks if `IsCompleted` is already true. If so, it doesn't need to update the `CompletedAt` timestamp unless the business logic requires tracking the 'latest' view.*
3. **How would you implement a "Continue Learning" button using this model?**
   *Answer: Query the `LessonProgress` for the current student, find the lesson with the highest `Order` that is NOT completed, and redirect the user there.*
