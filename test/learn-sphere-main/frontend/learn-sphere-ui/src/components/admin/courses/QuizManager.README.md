# Documentation: QuizManager.jsx

## Overview
`QuizManager.jsx` is a specialized editor for creating knowledge checks and exams.

## Detailed Explanation
This component handles the "Internal Geometry" of a quiz. It allows admins to:
- Set time limits and passing scores.
- Build multiple-choice questions.
- Dynamically add/remove options for each question.
- Mark which option is the correct answer using a radio-button style selection.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **5-16** | **Defaults**: Factory functions for clean question and quiz objects. |
| **23-32** | **Props & State**: Accepts `courseId` and `structure`; tracks quiz lists, active tabs, and builder forms. |
| **43-60** | **Lifecycle**: Fetches all existing quizzes for the specific chapter sequence using `Promise.all`. |
| **72-92** | `openEditQuiz`: Populates the builder form with existing data (supports JSON parsing for legacy data). |
| **94-108** | `addOrUpdateQuestion`: Staging logic; either replaces a question by index or appends it to the list. |
| **110-153** | `saveQuiz`: Validation checks followed by a Create/Update API call. |
| **183-238** | **Form UI**: Basic fields for Title, Time Limit, and Passing Score. |
| **309-401** | **Question Builder**: Textarea for the prompt and click-to-select radio inputs for answers. |
| **435-496** | **Question List**: Draggable summary of the current quiz structure with Edit/Delete buttons. |
| **549-582** | **Chapter Grid**: Entry point for each chapter to start building its own quiz list. |
| **590-643** | **Quiz List**: Summary of created quizzes per chapter with quick management toggles. |

## Program Flow
1. **Question Builder**: Admins type a question and fill in 4 potential answers.
2. **Correctness Selection**: Clicking an option marks it as the "Correct" one in the `current` question state.
3. **Staging**: Clicking "Add Question" pushes the current draft into the `form.questions` array.
4. **Persistence**: Clicking "Save Quiz" sends the entire package of questions to the `quizApi`.

## Why it is used
To provide a robust assessment system. It eliminates the need for admins to write JSON manually by providing a user-friendly interface for question construction.

## Interview Questions
1. **How do you handle the "Edit Mode" for an individual question within the quiz?**
   *Answer: By using an `editQIdx` state. When you click "Edit" on a question in the list, we copy that question's data into the builder and store its index. When you click "Update", we use that index to replace the old question in the array instead of adding a new one.*
2. **Why do we use `JSON.parse` on questions when editing an existing quiz?**
   *Answer: Data Compatibility. Some older parts of the system might store options as a JSON string. This ensures that no matter how the backend sent the data, the frontend always works with a clean JavaScript array.*
3. **Explain how the "Correct Answer" logic is visualized.**
   *Answer: It uses a "controlled" selection. Each option `div` has an `onClick` that updates `correctIndex`. The UI then conditionally applies a border color (`#6366f1`) and a "CORRECT" label to the selected index.*
