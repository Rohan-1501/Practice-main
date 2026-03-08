# Documentation: Assessment.cs

## Overview
`Assessment.cs` represents a formal evaluation, typically a "Final Exam", that a student must pass to complete a course.

## Detailed Explanation
Unlike informal quizzes, Assessments are tied directly to the `Course`. They include strict parameters like `TimeLimitMinutes`, `PassingScorePercentage`, and `MaxAttempts`.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for validation and schema mapping attributes. |
| **6** | Defines the `Assessment` class for high-stakes exams. |
| **9** | `Id`: Primary key for the assessment. |
| **12-15** | **Parent Link**: Connects the assessment to a specific `Course`. |
| **18** | `Title`: The display name (default: "Final Assessment"). |
| **20** | `Description`: Contextual info about the exam format and scope. |
| **22** | `TimeLimitMinutes`: Countdown timer value for the student (default: 30). |
| **24** | `PassingScorePercentage`: Numerical target (e.g., 70) to pass the exam. |
| **26** | `MaxAttempts`: Threshold for how many times a student can retake the exam. |
| **28** | `AccessDurationDays`: Optional window of availability after course start. |
| **31-33** | **Auditing**: Standard fields for tracking metadata updates. |
| **36** | **Navigation**: Collection of `AssessmentQuestions` that make up the test. |
| **37** | **Navigation**: Collection of all `AssessmentAttempts` made by students. |

## Program Flow
1. **Creation**: An instructor sets up an Assessment for a course.
2. **Eligibility**: When a student finishes all lessons, the Assessment becomes available on their dashboard.
3. **Execution**: The student starts an attempt. the system tracks time remaining and score.
4. **Grading**: On submission, the score is compared against `PassingScorePercentage` to determine if the student passed.

## Connections
- **Connected to `Course`**: Parent Course.
- **Connected to `AssessmentQuestion`**: contains the set of questions to be answered.
- **Connected to `AssessmentAttempt`**: Tracks individual student completions.

## Properties Explanation
- `TimeLimitMinutes`: The duration the student has to finish the exam.
- `PassingScorePercentage`: The minimum score (e.g., 70) required to "Pass".
- `MaxAttempts`: The number of times a student can retake the exam if they fail.
- `AccessDurationDays`: Optional field to limit how long the assessment is available after enrollment.

## Why it is used
It provides a high-stakes testing mechanism. It ensures that students have actually mastered the course material before receiving credit or a certificate.

## Interview Questions
1. **What is the difference between an Assessment and a Quiz in this system?**
   *Answer: Assessments are tied to the entire Course and usually have higher stakes (pass/fail for certificate), whereas Quizzes are tied to specific Chapters and are for modular learning.*
2. **How do you handle the "Time Out" scenario in code?**
   *Answer: The frontend usually has a timer, but the backend must also check the difference between `StartedAt` and the current time upon submission to ensure the `TimeLimit` wasn't exceeded.*
3. **Why use an `ICollection` for Questions and Attempts?**
   *Answer: To allow EF Core to load all related data using `.Include()` for easier reporting and grading logic.*
