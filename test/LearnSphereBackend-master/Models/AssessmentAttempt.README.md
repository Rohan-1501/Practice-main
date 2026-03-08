# Documentation: AssessmentAttempt.cs

## Overview
`AssessmentAttempt.cs` records a specific instance of a student taking an assessment.

## Detailed Explanation
It acts as a historical log for every time a student tries the exam. it tracks the score, whether they passed, and the status (Started, Completed, or TimedOut).

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for validation and schema mapping attributes. |
| **6** | Defines the `AssessmentAttempt` class for session tracking. |
| **9** | `Id`: Primary key for this specific exam session. |
| **12-15** | **Student Link**: Foreign key to the person taking the test. |
| **18-21** | **Assessment Link**: Foreign key identifying which exam is being taken. |
| **23** | `AttemptNumber`: Counter for retake enforcement (e.g., 1 of 2). |
| **25** | `Score`: The percentage achieved (null if currently in progress). |
| **27** | `Passed`: Boolean result calculated after the attempt ends. |
| **30** | `Status`: Enum-like string (Started, Completed, TimedOut). |
| **32** | `StartedAt`: The precise moment the countdown clock began. |
| **34** | `CompletedAt`: The moment the student clicked submit or was timed out. |

## Program Flow
1. **Start**: A new record is created with `Status = "Started"` and `StartedAt` timestamped.
2. **Submit**: When the student finishes, `Score` and `Passed` are calculated, `Status` becomes "Completed", and `CompletedAt` is set.
3. **Retry**: If the student failed and `AttemptNumber < MaxAttempts`, they can start a new attempt record.

## Connections
- **Connected to `Student`**: Identifies who is taking the test.
- **Connected to `Assessment`**: Identifies which test is being taken.

## Properties Explanation
- `AttemptNumber`: Incremented integer to track retries.
- `Score`: The percentage achieved (0-100).
- `Passed`: Boolean result based on the passing score.
- `Status`: Current state of the attempt.

## Why it is used
It provides auditability and allows for "Infinite" or "Limited" retries. It prevents a student from losing their progress if their computer crashes mid-way (the "Started" status remains).

## Interview Questions
1. **How do you prevent a student from taking more than the `MaxAttempts`?**
   *Answer: Before creating a new `AssessmentAttempt`, the controller should query the database for the count of existing attempts for that `StudentId` and `AssessmentId`.*
2. **What does the "TimedOut" status represent?**
   *Answer: It indicates that the student started the exam but did not submit it within the allowed time limit or within a reasonable grace period.*
3. **Why is `Score` nullable (`float?`)?**
   *Answer: It is null while the attempt is in the "Started" state and only gets a value once the attempt is "Completed".*
