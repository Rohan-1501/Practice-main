# Documentation: QuizQuestion.cs

## Overview
`QuizQuestion.cs` defines the questions for a quiz. It is a simpler version of the assessment question model, focused primarily on Multiple Choice (MCQ).

## Detailed Explanation
It stores the question text and a JSON array of options. It uses a single `CorrectIndex` because quizzes in this system primarily use single-choice questions.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Standard imports for data integrity and mapping. |
| **6** | Defines the `QuizQuestion` class. |
| **9** | `Id`: Unique primary key for the question. |
| **12-15** | **Hierarchy**: Links the question to its parent `Quiz`. |
| **18** | `Text`: The question prompt. |
| **21** | `Options`: **JSON Blob** string storing the possible answer texts. |
| **24** | `CorrectIndex`: The zero-indexed position of the right answer. |
| **26** | `Order`: Integer for sequential display on the lesson player. |

## Connections
- **Connected to `Quiz`**: parent Quiz.

## Properties Explanation
- `Options`: JSON string e.g., `["Red", "Blue", "Green"]`.
- `CorrectIndex`: The index of the right answer (e.g., `1` for "Blue").

## Interview Questions
1. **Why use `int CorrectIndex` instead of a string for the answer?**
   *Answer: It's more resilient to typos. If the option text changes (e.g., "Blue" to "Light Blue"), the index remains valid.*
2. **How do you display these questions on the frontend?**
   *Answer: Fetch the JSON, parse it into an array, and map it to a list of radio buttons.*
3. **What is the purpose of the `Order` property?**
   *Answer: To allow the instructor to control the sequence of questions rather than showing them in the order they were inserted into the DB.*
