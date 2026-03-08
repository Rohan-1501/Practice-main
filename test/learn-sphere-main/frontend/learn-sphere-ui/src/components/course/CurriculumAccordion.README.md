# Documentation: CurriculumAccordion.jsx

## Overview
`CurriculumAccordion.jsx` is the "Navigation Backbone" of the course experience. It presents the syllabus in an organized, collapsible format.

## Detailed Explanation
This component is modelled after the Udemy sidebar. It is a highly "Informative" component that shows:
- **Hierarchy**: Groups content into "Sections" (Chapters) and "Items" (Lessons/Quizzes).
- **Progress Visibility**: Uses icons to show if a student has completed a lesson or passed a quiz.
- **Stateful Interaction**: Remembers which chapters the user has opened and which lesson is currently "Active".
- **Dynamic Content**: Automatically picks the right icon based on whether the content is a Video, Document, Image, or Audio file.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1-13** | `import ... from 'lucide-react'` | **UI**: Imports relevant icons for Video, Audio, and Quiz types. |
| **31-38** | `const [openChapters, setOpen] = ...` | **State**: Dictionary tracking which chapters are expanded. |
| **40-53** | `getIcon = (type) => { ... }` | **Logic**: Returns a colored icon component based on file type. |
| **72-90** | `chapters.map(chapter => ...)` | **UI**: Renders the toggleable header for each course chapter. |
| **95-105** | `chapter.lessons.map(lesson => ...)` | **UI**: Renders the clickable rows for individual lessons. |
| **115-131** | `isLessonCompleted = (...)` | **Logic**: Compares `completedMap` vs total contents to show checks. |
| **140-150** | `lesson.contents.map(content => ...)` | **UI**: Lists sub-materials (e.g. Reading, Video) within a lesson. |
| **194-210** | `chapter.quizzes.map(quiz => ...)` | **UI**: Adds a dedicated entry for chapter-end knowledge checks. |
| **215-225** | `onQuizClick(quiz)` | **Event**: Notifies the player to load a specific quiz session. |

## Program Flow
1. **Sort Logic**: Automatically sorts chapters and lessons by their `order` property to ensure the syllabus is in the right sequence.
2. **Toggle Control**: Manages a local `openChapters` state so the user can collapse sections they have already finished.
3. **Completion Calculation**: For every lesson, it checks the `completedMap` to see if all sub-contents have been viewed. If yes, it renders a green `FaCheckCircle`.
4. **Context Callbacks**: When a user clicks an item, it fires the `onLessonClick` or `onQuizClick` function, telling the parent "The student wants to study this now."

## Why it is used
To provide "Structure". A course with 50 clips is overwhelming; an accordion allows students to see the "Big Picture" while focusing on one small section at a time.

## Interview Questions
1. **How do you determine if a lesson is "Completed" in the UI?**
   *Answer: Array Comparison. We look at the `completedMap` for that lesson's ID. We count how many content parts the lesson has, and if the count of "Completed Indices" in the map is equal to or greater than the total parts, we mark the lesson as done.*
2. **Why is `toggleChapter` using the function version of `setOpenChapters`?**
   *Answer: State atomicity. Since a user might click multiple chapters quickly, using `(prev) => ({ ...prev, [id]: !prev[id] })` ensures we are always toggling the latest state and not accidentally overwriting another chapter's toggle status.*
3. **Explain the `getIcon` utility function.**
   *Answer: Visual Consistency. It maps a string (like "video") to a specific React Icon component. This ensures that every video in the entire application always has the same indigo play icon, creating a cohesive design language.*
  
