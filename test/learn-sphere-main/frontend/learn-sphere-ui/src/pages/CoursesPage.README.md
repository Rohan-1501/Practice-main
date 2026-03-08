# Documentation: CoursesPage.jsx

## Overview
`CoursesPage.jsx` is the central catalog where students can explore and enroll in lessons.

## Detailed Explanation
This page is "Reactive". It uses an external state manager (`EnrollmentStore`) to track the user's progress in real-time. If a user enrolls in a course, the "Join Now" button instantly changes to "Continue" without the user needing to refresh the page.

## Program Flow
1. **Sync**: Uses `useSyncExternalStore` to listen for enrollment changes globally.
2. **Compare**: For every course in the database, it checks if that ID exists in the `enrolled` array.
3. **Adaptive UI**:
   - If "Completed" -> Show "Review Course".
   - If "Enrolled" -> Show "Continue".
   - If "New" -> Show "Join Now".

## Why it is used
To provide a smooth, interactive selection process. It gives the student a clear view of their learning progress across the entire platform.

## Interview Questions
1. **What is `useSyncExternalStore` and why is it used here instead of `useContext`?**
   *Answer: It is a React 18 hook used for subscribing to data sources outside of React's state (like `localStorage` or a custom store). It prevents "tearing" (UI glitches) and is more performant than context for frequently changing global data.*
2. **How do you handle courses with missing levels or thumbnails?**
   *Answer: Conditional Rendering and Default Props. We use `course.level || "N/A"` to prevent empty text and `course.thumbnail || "/assets/placeholder.jpg"` to ensure every card has a professional visual, even if the image upload failed.*
3. **Explain the `line-clamp-2` utility used on the title.**
   *Answer: UI Layout Stability. If a course has a very long title, it might break the grid's alignment. `line-clamp-2` ensures the title takes up exactly two lines (ending with "...") keeping all cards perfectly aligned.*
