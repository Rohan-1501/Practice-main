# Documentation: Quiz.cs

## Overview
`Quiz.cs` represents a short knowledge check typically found at the end of a Chapter.

## Detailed Explanation
Quizzes are less formal than Assessments. They are tied to a `Chapter` rather than the whole `Course` and are used for incremental learning validation. They still include a time limit and passing score.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for schema constraints and relationship mapping. |
| **6** | Defines the `Quiz` class for modular knowledge checks. |
| **9** | `Id`: Primary key for the quiz. |
| **12-15** | **Parent Link**: Connects the quiz to a specific `Chapter`. |
| **18** | `Title`: The name of the quiz (e.g., "Intro Quiz"). |
| **20** | `Description`: brief context for the student. |
| **22** | `TimeLimitMinutes`: Countdown timer value (default: 15). |
| **24** | `PassingScorePercentage`: Benchmark for completion (default: 60). |
| **26** | `Order`: Sequence position within the chapter contents. |
| **28-30** | **Auditing**: Standard fields for tracking creation and updates. |
| **33** | **Navigation**: Collection of `QuizQuestions` belonging to this quiz. |

## Program Flow
1. **Integration**: As a student finishes a chapter, they encounter the Quiz.
2. **Validation**: Passing the quiz might be a prerequisite for unlocking the next chapter.
3. **Ordering**: Quizzes can be placed at specific points in the curriculum using the `Order` property.

## Connections
- **Connected to `Chapter`**: Parent Chapter.
- **Connected to `QuizQuestion`**: Contains the quiz questions.

## Properties Explanation
- `TimeLimitMinutes`: Usually shorter than an assessment (e.g., 15 mins).
- `PassingScorePercentage`: The target for successful completion.

## Why it is used
It provides immediate feedback to students. It breaks the "Passive" watching/reading flow with an "Active" testing component.

## Interview Questions
1. **Why is `Quiz` connected to `Chapter` while `Assessment` is connected to `Course`?**
   *Answer: This reflects a hierarchical educational structure: Chapter-level quizzes test modular knowledge, while Course-level assessments test comprehensive knowledge.*
2. **What happens if you delete a Chapter?**
   *Answer: Due to the foreign key relationship, the associated Quiz (and its questions) should typically be deleted or re-assigned to avoid orphaned records.*
3. **How does the `Order` property interact with Lessons?**
   *Answer: In a sophisticated UI, both Lessons and Quizzes are sorted by their `Order` values together to create a mixed content list.*
