# Documentation: QuizzesForm.jsx

## Overview
`QuizzesForm.jsx` is a lightweight, internal utility for building per-chapter knowledge checks.

## Detailed Explanation
While the "Assessment" is for the end of the course, "Quizzes" are for the end of a chapter. This component provides a simplified version of the question builder specifically for these smaller tests. It allows admins to set a time limit and a passing score for each specific quiz.

## Program Flow
1. **ID Generation**: Uses `Date.now()` to generate a unique ID for every new question, ensuring that the React `.map` function has a stable `key` to work with.
2. **Syncing**: Instead of managing its own API calls, this component "Syncs" its state back to its parent using a `syncWithParent` callback. This makes it highly modular.
3. **Visual Feedback**: It uses a "Correct Answer" badge to help the instructor see at a glance which option is the intended choice.

## Why it is used
To reinforce learning at a granular level. By placing quizzes after chapters, the platform helps students "Lock In" what they just learned before moving to more difficult material.

## Interview Questions
1. **Why use `Date.now()` for question IDs?**
   *Answer: Quick Uniqueness. Since these questions aren't saved to the database until the "Finalize" button is clicked, we need a way to track them in our local array. `Date.now()` is a reliable way to get a unique number for every new item created during the session.*
2. **What is the `cardStyle` object doing here?**
   *Answer: Inline Styling. While most of the app uses Tailwind, this component uses an inline JS object to define its background and borders. This is a common pattern for "Utility" components that need to be portable across different parts of the platform.*
3. **Why does the `onChange` for the options use `e.stopPropagation()`?**
   *Answer: Event Bubbling. Clicking on an input field also counts as clicking on the "Row" (which selects it as the correct answer). `stopPropagation` ensures that typing in the box doesn't accidentally change which answer is marked as correct.*
  
