# Documentation: QuizAttempt.cs

## Overview
`QuizAttempt.cs` tracks a student's performance on a specific quiz.

## Detailed Explanation
Similar to `AssessmentAttempt`, it records the score and whether the student passed. however, it is usually simpler as quizzes often allow unlimited retries in this system.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for validation and schema mapping attributes. |
| **6** | Defines the `QuizAttempt` class for session tracking. |
| **9** | `Id`: Primary key for this specific quiz session. |
| **12-15** | **Student Link**: Identifies the test-taker. |
| **18-21** | **Quiz Link**: Identifies which quiz is being taken. |
| **23** | `Score`: The percentage achieved (0-100). |
| **25** | `Passed`: Boolean result indicating completion. |
| **27** | `AttemptedAt`: The timestamp of the activity. |

## Connections
- **Connected to `Student`**: The test-taker.
- **Connected to `Quiz`**: The specific quiz.

## Why it is used
It allows students to see their past performance on modular topics and helps instructors identify which chapters students are struggling with the most.

## Interview Questions
1. **How is this model used in a "Progress Bar"?**
   *Answer: The system can check if at least one `QuizAttempt` with `Passed = true` exists for every quiz in the course.*
2. **Why is `AttemptedAt` defaulted to `DateTime.UtcNow`?**
   *Answer: To ensure consistency across different server locations and simplify time-difference calculations for "Recently Attempted" lists.*
3. **Does this model store the student's individual answers?**
   *Answer: No, it only stores the final result. If you needed to review specific answers, you would need a child model like `QuizAnswer`.*
