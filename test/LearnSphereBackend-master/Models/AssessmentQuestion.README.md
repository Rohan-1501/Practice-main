# Documentation: AssessmentQuestion.cs

## Overview
`AssessmentQuestion.cs` defines an individual question within an assessment, including its options and correct answers.

## Detailed Explanation
The model supports multiple question types like Multiple Choice (MCQ) and Multiple Select. It uses JSON-serialized strings to store options and correct indices, making it flexible for different question formats.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Standard imports for data integrity and mapping. |
| **6** | Defines the `AssessmentQuestion` class. |
| **9** | `Id`: Unique primary key for the question. |
| **12-15** | **Hierarchy**: Links the question to its parent `Assessment` template. |
| **18** | `Text`: The actual question prompt (e.g., "What is React?"). |
| **21** | `Type`: Discriminator for UI logic (Single Choice vs Multi-Select). |
| **24** | `Options`: **JSON Blob** string storing the list of possible answers. |
| **30** | `CorrectIndices`: **JSON Blob** string storing the zero-indexed answer keys. |
| **32** | `Order`: Integer for sequential display on the assessment player. |

## Program Flow
1. **Definition**: Admin creates questions and stores options as a JSON array (e.g., `["A", "B", "C"]`).
2. **Ordering**: Questions are sorted using the `Order` property when presented to the student.
3. **Validation**: When the student submits their answers, the backend parses `CorrectIndices` to score the response.

## Connections
- **Connected to `Assessment`**: Belongs to a specific assessment.

## Properties Explanation
- `Text`: The actual question being asked.
- `Type`: "MCQ" (one answer) or "MultipleSelect" (multiple answers).
- `Options`: A JSON string representing the possible answers.
- `CorrectIndices`: A JSON string representing the zero-based index of the correct answer(s).

## Why it is used
It centralizes question logic. By using JSON strings for options/answers, the database schema stays simple even if questions have 2 options or 10.

## Interview Questions
1. **What are the pros and cons of storing options as a JSON string?**
   *Answer: Pros: Simple schema, no need for an "Options" table. Cons: Harder to query specific options via raw SQL and requires serialization/deserialization logic in the app code.*
2. **How does `CorrectIndices` handle Multiple Select questions?**
   *Answer: It stores an array of indices, e.g., `[0, 2]`. The logic then checks if the student's selected indices match this array perfectly.*
3. **Why is the `AssessmentId` marked as `[Required]`?**
   *Answer: To enforce referential integrity; a question cannot exist without being part of an assessment.*
