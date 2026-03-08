# Documentation: PAcademic.jsx

## Overview
`PAcademic.jsx` manages the student's institutional and academic data.

## Detailed Explanation
Similar to the Personal Info component, this sub-form focuses on Roll Numbers, Courses, and Years. It uses the same "Draft Persistence" pattern, ensuring that academic details are synced with the user's local browser storage.

## Program Flow
1. **State Initialization**: Uses a "Lazy" init function inside `useState` (e.g., `() => localStorage.getItem(...)`) to load data instantly on the first render.
2. **Persistence**: Syncs Roll Number, Course, and Year to `localStorage` via `useEffect`.
3. **Input Constraints**: The "Year" input is strictly limited to a range of 1–5 using the `min` and `max` attributes.

## Why it is used
To categorize students for analytics and certification. These details are used by the backend to identify which cohort a student belongs to.

## Interview Questions
1. **What is the benefit of the `() => localStorage.getItem(...)` syntax inside `useState`?**
   *Answer: This is called "Lazy Initial State". It ensures that the heavy task of reading from `localStorage` only happens once (on mount). In subsequent re-renders, React ignores this function, which improves performance.*
2. **Explain the `required` attribute on the inputs.**
   *Answer: This is HTML5 validation. It prevents the user from submitting the profile form if these critical academic fields are empty, showing a "Please fill out this field" tooltip automatically.*
3. **Why use a `<select>` for the Course instead of a text input?**
   *Answer: Data consistency. By providing a dropdown, we ensure that students don't make typos (like "CS" vs "Computer Science"), which makes it much easier for admins to filter students on the backend.*
