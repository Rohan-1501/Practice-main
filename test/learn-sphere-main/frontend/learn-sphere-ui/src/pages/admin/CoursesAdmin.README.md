# Documentation: CoursesAdmin.jsx

## Overview
`CoursesAdmin.jsx` is the most sophisticated "State Machine" in the application, orchestrating the 4-stage course creation process.

## Detailed Explanation
Instead of a single giant form, this page breaks course creation into a logical workflow using a tabbed interface.
1. **Course Details**: Basic info (managed by `CourseForm`).
2. **Structure**: Chapter and Lesson builder (managed by `CourseStructureManager`).
3. **Assessment**: The "Final Exam" (managed by `AssessmentForm`).
4. **Quizzes**: Small check-ins inside chapters (managed by `QuizManager`).

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **20-25** | `const CATEGORY_OPTIONS = ...` | **Data**: Centralized list of academic categories for course classification. |
| **30** | `editing, setEditing] = useState(null)` | **State**: Tracks whether the UI is in list mode, editing mode, or "New Course" mode. |
| **35-47** | `const [form, setForm] = ...` | **State**: Complex form object with validation defaults for every course property. |
| **55-62** | `useEffect(() => { ... })` | **Lifecycle**: Initial bootstrap that populates the management list from the API. |
| **64-74** | `onEdit = (course) => { ... }` | **Logic**: Pre-loads course metadata and asynchronously triggers a structure fetch. |
| **78-80** | `useEffect(() => { ... })` | **Logic**: Specialized tab-listener that refreshes Quiz/Assessment data on switch. |
| **108-115** | `onSave = async () => { ... }` | **UX**: Orchestrates validation and saves metadata to the persistent backend. |
| **124** | `createCourse({ ... })` | **Async**: POST request that registers a brand new course in the system. |
| **130-134** | `setActiveTab("structure")` | **UX**: Automatically transitions the user from "Details" to "Structure" on success. |
| **141-149** | `onFinalize = async () => { ... }` | **Logic**: Multi-save orchestrator that commits the entire course blueprint at once. |
| **200-217** | `[...].map((tab) => ...)` | **UI**: Dynamic tab-bar that controls the four-stage course creation wizard. |
| **222** | `<CourseForm ... />` | **Sub**: Specialized component for rich text description and metadata editing. |
| **250** | `<CourseStructureManager ... />` | **Sub**: Advanced recursive component for building chapters and lesson trees. |
| **293** | `<QuizManager ... />` | **Sub**: Dedicated builder for chapter-end knowledge checks. |

## Why it is used
To simplify a highly complex task. Building a 20-hour course is daunting; breaking it into these 4 discrete steps makes it manageable for instructors.

## Interview Questions
1. **How do you persist the "Draft" state when switching between tabs?**
   *Answer: By hoisting the state to the parent. The `CoursesAdmin` page holds the `form` and `courseStructure` in its own state. When you switch tabs, those sub-components are swapped out, but the data remains safe in the parent's memory.*
2. **Explain the logic in the "Create & Next" transition.**
   *Answer: After the `createCourse` API returns a success, we update the `editing` state with the new ID and immediately switch `activeTab` to "structure". This creates a "Natural Flow" for the instructor.*
3. **Why do you use `useRef` for `titleRef`?**
   *Answer: Focus Management. When an admin starts a "New Course", we use the ref to automatically put the cursor in the Title field. This one extra detail makes the software feel "Snappy" and high-quality.*
  
