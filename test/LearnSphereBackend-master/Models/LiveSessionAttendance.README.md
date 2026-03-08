# Documentation: LiveSessionAttendance.cs

## Overview
`LiveSessionAttendance.cs` tracks which students actually joined a specific live session.

## Detailed Explanation
It records the "Who" and "When" of attendance for synchronous events. This data is often used for compliance or to award participation points.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for property attributes. |
| **6** | Defines the `LiveSessionAttendance` class. |
| **9** | `Id`: Primary key for the attendance log. |
| **12-15** | **Session Link**: Foreign key for the specific event attended. |
| **18-21** | **Student Link**: Foreign key identifying the attendee. |
| **23** | `JoinedAt`: Timestamp of the click-to-join event. |

## Connections
- **Connected to `LiveSession`**: The event being attended.
- **Connected to `Student`**: The student attending.

## Why it is used
It provides a way to measure engagement in live events. Without this, instructors would have no way of knowing how many of their enrolled students are actually participating in the live parts of the course.

## Interview Questions
1. **What is the primary key of this model?**
   *Answer: The `Id` property, which is an auto-incrementing integer.*
2. **How would you calculate the "Popularity" of a live session?**
   *Answer: By counting the number of `LiveSessionAttendance` records associated with that `LiveSessionId`.*
3. **Wait, why is `JoinedAt` defaulted to `UtcNow`?**
   *Answer: To record the exact moment the student clicked the "Join" button or accessed the live stream.*
