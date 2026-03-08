# Documentation: CourseContentViewer.jsx

## Overview
`CourseContentViewer.jsx` is a comprehensive "Preview" modal for course reviewers and admins.

## Detailed Explanation
It acts as a multi-perspective lenses for a course. Instead of having to walk through the entire editing process, an admin can use this component to quickly "Check" everything. It uses a Tabbed Interface:
- **Course Details**: Basic info (Price, Status, Slug).
- **Course Structure**: Visualization of chapters and lessons.
- **Assessment**: View the final exam and its questions.
- **Quizzes**: View all chapter-level knowledge checks.

## Program Flow
1. **Lazy Loading**: To save performance, it only fetches "Structure" or "Quiz" data once the user clicks on those specific tabs.
2. **State Management**: It maintains local "Caches" of the structure and quizzes so that switching between tabs is instantaneous after the first load.
3. **Formatting**: It uses utility functions like `formatFileSize` to make technical data (like content size) look professional (e.g., "1.5 MB" instead of "1500000").

## Why it is used
It is the "Final Proofing" tool. It allows admins to see exactly how a course is organized before they decide to "Publish" it to students.

## Interview Questions
1. **Explain the benefits of the "Lazy Loading" pattern in the `useEffect` hooks.**
   *Answer: Efficiency. A course might have hundreds of lessons and dozens of quizzes. If we loaded everything as soon as the modal opened, the browser might freeze. By checking `if (activeTab !== "structure") return;`, we ensure we only use bandwidth when the user actually needs that data.*
2. **How does this component handle the display of multiple "Correct Indices" in Assessments?**
   *Answer: Unlike Quizzes (which usually have 1 correct answer), Assessments in this platform support "Multi-Select" questions. The UI uses `q.correctIndices?.includes(optIdx)` to highlight every option that is marked as correct.*
3. **Why is the the `bg-gradient-to-br` used on the modal container?**
   *Answer: Aesthetics. It gives the modal a premium, "Glassy" look that aligns with the rest of the dark-themed platform, making the admin experience feel high-end.*
