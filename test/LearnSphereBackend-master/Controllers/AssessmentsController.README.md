# Documentation: AssessmentsController.cs

## Overview
`AssessmentsController.cs` manages the high-stakes "Final Exams" for each course.

## Detailed Explanation
This controller contains some of the most advanced logic in the platform, including:
- **Eligibility Computing**: Verifies that a student has finished ALL lessons and passed ALL chapter quizzes before they can start the exam.
- **Time Limits**: Automatically fails ("TimedOut") attempts that exceed the allowed duration.
- **Scoring**: Compares student answers against a JSON-serialized list of correct indices.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **11-12** | `[ApiController] [Route("api/Assessments")]` | **Meta**: Defines the API and its primary certification endpoint. |
| **15-16** | `public AssessmentsController(...)` | **Constructor**: Injects the Context for direct DB orchestration. |
| **23-33** | `GetByCourse(int courseId)` | **API**: Public GET for the course's final assessment shell. |
| **36-37** | `[HttpPut("course/{id}")] [Authorize]` | **API**: Powerful "Upsert" logic for admins to define the exam. |
| **53-58** | `assessment.Title = req.Title ...` | **Logic**: Maps the configuration properties to the DB record. |
| **63** | `await UpsertQuestions(...)` | **Recursive**: Synchronizes the question bank for the exam. |
| **77-88** | `[HttpDelete("course/{id}")]` | **API**: Safely wipes the assessment and clears notifications. |
| **99-100** | `[HttpGet("course/{id}/eligibility")]` | **API**: The "Gatekeeper" that checks if a student is ready. |
| **106** | `ComputeEligibilityAsync(...)` | **Engine**: Recursive logic checking all Lessons and Quizzes. |
| **109-130** | `_db.Notifications.Any(...)` | **Logic**: Prevents spam by checking if an "Unlocked" alert exists. |
| **166-167** | `[HttpPost("course/{id}/start")]` | **API**: Student marks the start of their timed session. |
| **182** | `if (count >= exam.MaxAttempts)` | **Guard**: Denies access if the student has exhausted their tries. |
| **191-203** | `new AssessmentAttempt { ... }` | **Logic**: Records the `StartedAt` time on the server to prevent cheating. |
| **206-215** | `questions.Select(q => ...)` | **Security**: High-impact mapping that **hides correct answers** from UI. |
| **227-228** | `[HttpPost("attempt/{id}/submit")]` | **API**: Critical grading and course completion endpoint. |
| **246-254** | `if (DateTime.UtcNow > deadline)` | **Guard**: Implements a strict time-out based on the original `StartedAt`. |
| **259-284** | `foreach (var q in exam.Questions)` | **Grading**: Compares submission keys to the secure DB answer keys. |
| **295-341** | `if (passed) { ... }` | **Lifecycle**: Marks the course as "Completed" in the enrollment. |
| **390-391** | `[HttpPost("lesson/{id}/complete")]` | **API**: Standard "I've read this" tracker for curriculum items. |
| **402** | `await _db.LessonProgresses.AddAsync(...)` | **Persistence**: Records the unit completion in the database. |
| **412** | `await ComputeEligibilityAsync(...)` | **Sync**: Immediate check to see if the exam just unlocked. |
| **645-700** | `ComputeEligibilityAsync(...)` | **Core**: The SQL-heavy engine matching progress vs total counts. |

## Program Flow
1. **Eligibility Check**: Student asks "Can I take the test?". Controller calculates their progress.
2. **Start Attempt**: Creates a "Started" record and provides questions (with correct answers hidden).
3. **Submission**: Validates time, calculates score, updates high score in the `Enrollment` table, and marks the course as "Completed" if they passed.
4. **Certification Logic**: Updates "Compliance" status based on passing scores and due dates.

## Connections
- **Interacts with `AppDbContext` directly** (unlike others that use repositories).
- **Triggers notifications** via `NotificationsController` when a student becomes eligible.

## Why it is used
It provides the "Final Validation" for the learning process. It ensures that students don't just "Watch videos" but actually demonstrate knowledge before getting credit.

## HTTP GET Methods: Detailed Explanation

### 1. `GetByCourse(int courseId)`
- **Endpoint**: `GET /api/Assessments/course/{courseId}`
- **Working**: Fetches the assessment configuration (title, time limit, passing score) and all associated questions for a specific course. It uses `.Include(a => a.Questions)` to ensure all data is returned in a single database round-trip.

### 2. `CheckEligibility(int courseId)`
- **Endpoint**: `GET /api/Assessments/course/{courseId}/eligibility`
- **Working**: This is a "Gatekeeper" method. It calculates if a student has met all prerequisites (completing all lessons and passing all chapter quizzes). It returns a detailed object explaining why a student is or is not allowed to start the final exam.

### 3. `GetMyAttempts(int courseId)`
- **Endpoint**: `GET /api/Assessments/course/{courseId}/my-attempts`
- **Working**: Retrieves a history of all attempts the current student has made for this assessment. This allows the student to see their previous scores and determine if they need to use one of their remaining attempts.

### 4. `GetProgress(int courseId)`
- **Endpoint**: `GET /api/Assessments/course/{courseId}/progress`
- **Working**: Returns a simple list of `completedLessonIds` and `passedQuizIds`. This is used by the frontend to highlight completed items in the curriculum sidebar.

## HTTP POST, PUT & DELETE Methods: Detailed Explanation

### 1. `Upsert(int courseId)`
- **Endpoint**: `PUT /api/Assessments/course/{courseId}`
- **Access**: Admin Only
- **Working**: An "All-in-One" method for admins to create or update an exam. It saves the exam settings and replaces the entire list of questions in one transaction to ensure data consistency.

### 2. `Delete(int courseId)`
- **Endpoint**: `DELETE /api/Assessments/course/{courseId}`
- **Access**: Admin Only
- **Working**: Completely removes the exam from a course. It also triggers a cleanup of any notifications that were sent to students about this specific exam.

### 3. `Start(int courseId)`
- **Endpoint**: `POST /api/Assessments/course/{courseId}/start`
- **Working**: The "Entry Point" for students. It creates a new `AssessmentAttempt` record with a `StartedAt` timestamp and returns the question set. It enforces attempt limits and access window guards.

### 4. `Submit(int attemptId)`
- **Endpoint**: `POST /api/Assessments/attempt/{attemptId}/submit`
- **Working**: The core grading engine. It loops through student answers, compares them to correct keys, calculates the score, and updates the student's `Enrollment` status to "Completed" if they passed.

### 5. `CompleteLesson(int lessonId)`
- **Endpoint**: `POST /api/Assessments/lesson/{lessonId}/complete`
- **Working**: Marks a specific lesson as finished. Crucially, it re-calculates the student's overall course eligibility after every update to potentially trigger an "Assessment Unlocked" alert.

## Interview Questions
1. **How do you prevent a student from seeing the correct answers while taking the test?**
   *Answer: The `Start` endpoint manually filters out the `CorrectIndices` field when sending the questions list to the frontend.*
2. **Explain the "1-minute grace period" in the `Submit` method.**
   *Answer: It accounts for network latency. If a student's timer hits zero, it might take a few seconds for the request to reach the server. The grace period prevents unfair "TimedOut" failures due to slow internet.*
3. **What is the "Soft Idempotent Emit" logic used here?**
   *Answer: It ensures that an "Assessment Unlocked" notification is only sent ONCE to a student, no matter how many times they refresh their dashboard after becoming eligible.*
