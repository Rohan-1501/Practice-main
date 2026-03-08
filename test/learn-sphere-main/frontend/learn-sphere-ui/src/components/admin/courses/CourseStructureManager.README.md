# Documentation: CourseStructureManager.jsx

## Overview
`CourseStructureManager.jsx` is the engine for building the course curriculum tree.

## Detailed Explanation
This component allows admins to design the learning path. It follows a three-level hierarchy:
1. **Chapter**: The broad section (e.g., "Introduction").
2. **Lesson**: The specific topic inside a chapter (e.g., "What is React?").
3. **Content**: The actual files (MP4s, PDFs) attached to a lesson.

It manages local state for new chapter/lesson forms and communicates with the `courseApi` to persist these changes in the database.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-4** | Imports for React hooks, the Course API service, and title validation logic. |
| **6-10** | **Props**: Accepts the `courseId` and a flag (`isEmbedded`) to determine the visual layout. |
| **11-15** | **Core State**: Tracks the course data, loading status, errors, and the active upload modal. |
| **18-23** | **Creation State**: Buffered inputs for new chapters and lessons, including real-time error messages. |
| **25-31** | **Lifecycle**: Triggers the load of the full structural tree when the component mounts. |
| **33-44** | `loadCourseStructure`: Fetches the complex nested JSON from the backend. |
| **46-74** | `handleUploadContent`: Orchestrates the multipart/form-data upload to the server. |
| **76-86** | `handleDeleteContent`: Confirm and delete a specific resource (document/video). |
| **88-117** | `handleAddChapter`: Logic for creating a new section with order calculation. |
| **119-160** | `handleAddLesson`: Logic for adding a lesson to a specific parent chapter. |
| **162-200** | **State Helpers**: Updates input fields while triggering real-time title validation. |
| **202-212** | **Loading State**: Displays a spin icon while the tree is being fetched. |
| **215-223** | **Safety Check**: Prevents building structure if the course hasn't been saved yet. |
| **235-253** | **Container**: Responsive wrapper (Modal vs Embedded) with a close button. |
| **267-283** | **Chapter Loop**: Maps through chapters, showing titles and descriptions. |
| **288-322** | **Lesson Loop**: Maps through lessons; includes the "Upload" trigger button. |
| **325-356** | **Content List**: Displays icons and sizes for all files attached to a lesson. |
| **371-436** | **Lesson Form**: Inline inputs for adding new lessons to the current chapter. |
| **442-484** | **Chapter Form**: Dedicated area at the bottom for expanding the course. |
| **487-535** | **Empty State**: Friendly prompt shown when a course has zero chapters. |
| **553-562** | **Modals**: Renders the `ContentUploadModal` when a user clicks "Upload Content". |
| **567-575** | `getContentIcon`: Semantic helper for mapping file types to emojis. |
| **577-583** | `formatFileSize`: Utility to convert raw bytes into human-readable KB/MB strings. |

## Program Flow
1. **Loading**: Fetches the full course structure (including all nested chapters and lessons) on mount.
2. **Creation**: 
   - Uses `handleAddChapter` to create a new section.
   - Uses `handleAddLesson(chapterId)` to append a lesson to a specific section.
3. **Content Upload**: Spawns the `ContentUploadModal` to handle the physical file transfer via the backend's `FileUploadService`.

## Why it is used
It gives instructors a structured, visual way to organize complex educational material. Its real-time updates ensure that the "Build" process feels snappy and intuitive.

## Interview Questions
1. **How do you handle the "Recursive" nature of course data?**
   *Answer: By mapping through nested arrays. We iterate through `chapters`, and inside each chapter loop, we iterate through its `lessons`, and inside each lesson, we iterate through its `contents`. This pattern perfectly mirrors the backend's DTO structure.*
2. **Why do lessons have an `order` property?**
   *Answer: Sequence control. It allows admins to decide which lesson comes first. The frontend calculates the new order by checking the `length` of the current lesson array and adding 1.*
3. **How does this component prevent users from adding lessons to a non-existent course?**
   *Answer: It features a guard clause. If the `courseId` is "new", it renders a message telling the user to "Save the Course First" before building the curriculum.*
4. **Explain the `isEmbedded` prop.**
   *Answer: Reusability. This prop allows the manager to be used either as a standalone full-screen modal OR as a piece of another page (like the Course Editor). If `isEmbedded` is true, it hides its own header and footer.*
  
