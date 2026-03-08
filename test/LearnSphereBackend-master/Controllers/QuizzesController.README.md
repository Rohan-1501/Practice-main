# Documentation: QuizzesController.cs

## Overview
`QuizzesController.cs` handles regular knowledge checks at the end of chapters.

## Detailed Explanation
It is a simpler version of the `AssessmentsController`. It supports CRUD (Create, Read, Update, Delete) operations for admins and basic submission/tracking for students.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **8** | `[ApiController]` | **Attribute**: Enables automatic model state validation. |
| **9** | `[Route("api/Quizzes")]` | **Attribute**: Sets the base URL for the chapter-end quiz engine. |
| **11** | `public class QuizzesController ...` | Defines the quiz management and grading controller. |
| **13** | `private readonly AppDbContext _db;` | **Data**: Direct reference to the main database context. |
| **15** | `public QuizzesController(...)` | **Constructor**: Satisfies the class on the DB context. |
| **20-21** | `[HttpGet("chapter/{chapterId}")]` | **API**: Fetches quizzes associated with a specific section. |
| **24** | `_db.Quizzes.Where(...)` | **Logic**: Queries the DB for quizzes belonging to the requested chapter. |
| **51-52** | `[HttpPost] [Authorize(Roles = "admin")]` | **API**: Secure POST for admins to create new quiz templates. |
| **79-81** | `[HttpPut("{id}")] [Authorize(Roles = "admin")]` | **API**: High-impact update that replaces quiz questions. |
| **99-100** | `[HttpDelete("{id}")] [Authorize(Roles = "admin")]` | **API**: Removes a quiz and cascaded questions from the system. |
| **114-115** | `[HttpPost("{id}/submit")] [Authorize]` | **API**: The student-facing grading and submission endpoint. |
| **125-130** | `foreach (var q in questions)` | **Grading**: Compares student input against correct indices in real-time. |
| **135-143** | `new QuizAttempt { ... }` | **Persistence**: Records the student's score in the attempt history. |
| **169-180** | `UpsertQuestions(...)` | **Helper**: Wipes old questions and inserts new ones during updates. |
| **199-215** | `MapQuiz(...)` | **Map**: Converts DB entities into clean JSON objects for the UI. |
| **232-244** | **DTOs**: Defines the structure for creating, updating, and submitting quizzes. |

## Program Flow
1. **Creation**: Admin adds questions to a quiz. Questions are serialized as JSON.
2. **Submission**: Student submits a dictionary of `AnswerId -> SelectedIndex`. 
3. **Grading**: Backend compares selections against `CorrectIndex` and saves a `QuizAttempt` record.

## Why it is used
It facilitates "Micro-Learning" by reinforcing concepts immediately after they are presented in a chapter.

## HTTP GET Methods: Detailed Explanation

### 1. `GetByChapter(int chapterId)`
- **Endpoint**: `GET /api/Quizzes/chapter/{chapterId}`
- **Working**: Fetches all quizzes associated with a specific chapter. It is used to populate the "Quick Quiz" section at the end of a chapter's lesson list.

### 2. `GetById(int id)`
- **Endpoint**: `GET /api/Quizzes/{id}`
- **Working**: Retrieves the full quiz data including all questions and options. This is called by the `QuizPlayer` component to load the interactive exam interface.

### 3. `GetMyAttempt(int id)`
- **Endpoint**: `GET /api/Quizzes/{id}/my-attempt`
- **Working**: Checks if the student has already taken this quiz and returns their latest score and "Passed" status.

## HTTP POST, PUT & DELETE Methods: Detailed Explanation

### 1. `Create()`
- **Endpoint**: `POST /api/Quizzes`
- **Access**: Admin Only
- **Working**: Standard creation method. It maps the incoming DTO to a new `Quiz` object and saves it. It also processes the associated questions list immediately.

### 2. `Update(int id)`
- **Endpoint**: `PUT /api/Quizzes/{id}`
- **Access**: Admin Only
- **Working**: Updates the quiz settings and **completely replaces** the existing questions with the new set provided in the request body to maintain simplicity.

### 3. `Delete(int id)`
- **Endpoint**: `DELETE /api/Quizzes/{id}`
- **Access**: Admin Only
- **Working**: Removes the quiz record. The associated `QuizQuestion` records are automatically deleted by the database via foreign key constraints.

### 4. `Submit(int id)`
- **Endpoint**: `POST /api/Quizzes/{id}/submit`
- **Working**: The student-facing grading logic. It compares the selected indices against the `CorrectIndex` stored in the DB and records a new `QuizAttempt`.

## Interview Questions
1. **How is the question data stored in the database for a quiz?**
   *Answer: The `Options` list is serialized as a JSON string within the `QuizQuestion` record. This avoids a complex multi-table join and simplifies the retrieval logic.*
2. **What is the purpose of `Order` in the `GetByChapter` method?**
   *Answer: It ensures that if a chapter has multiple quizzes, they are shown in the specific sequence intended by the instructor.*
3. **Explain the `MapQuiz` helper method found in this controller.**
   *Answer: It is a "Manual Mapper". It converts the raw `Quiz` entity (with its DB-specific properties) into a cleaner anonymous object that the frontend is expecting, including deserializing JSON options.*
