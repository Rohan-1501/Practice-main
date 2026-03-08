# Documentation: AssessmentForm.jsx

## Overview
`AssessmentForm.jsx` is the tool used to create the final, high-stakes examination for a course.

## Detailed Explanation
Unlike simple quizzes, the "Assessment" is the final hurdle a student must pass to complete a course. This form allows admins to configure strict rules:
- **Passing Score**: Percentage needed to pass (e.g., 70%).
- **Time Limit**: Enforces a countdown during the exam.
- **Max Attempts**: Limits how many times a student can retake the exam.
- **Question Types**: Supports both Single Choice (MCQ) and Multiple Select (where the student must check all that apply).

## Program Flow
1. **Dynamic Options**: Each question defaults to 4 options, and the admin can toggle which ones are "Correct".
2. **Type Switching**: When switching types, the component automatically resets the `correctIndices` to ensure the data format remains valid.
3. **Upsert Logic**: It uses an "Upsert" (Update or Insert) pattern—if an assessment already exists for the course, it overwrites the old one; otherwise, it creates a new one.

## Why it is used
To provide a formal certification mechanism. By allowing for complex question types and strict attempt limits, it ensures that students actually understand the material before they are marked as "Completed".

## Interview Questions
1. **How do you handle the "Multiple Select" logic when a user clicks an option?**
   *Answer: By using a toggle pattern. We check if the clicked index is already in the `correctIndices` array. If it is, we `filter` it out. If it isn't, we add it using the spread operator `[...prev, i]`. This allows the admin to select any number of correct answers.*
2. **Why do we use `number` inputs for time limits and scores?**
   *Answer: Browser-level validation. By setting `type="number"` and `min={0}`, we prevent the admin from accidentally entering text or negative numbers, which would break the backend's scoring logic.*
3. **Explain the benefit of the `editIdx` state.**
   *Answer: It allows for an "Inline Editing" experience. Instead of the admin having to delete and recreate a question to fix a typo, they can click "Edit", which loads the question back into the builder. The `editIdx` tells the "Save" function to replace the existing item in the array rather than adding a new one.*
  
