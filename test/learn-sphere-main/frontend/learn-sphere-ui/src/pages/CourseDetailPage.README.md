# Documentation: CourseDetailPage.jsx

## Overview
`CourseDetailPage.jsx` is the "Storefront" view for an individual course.

## Detailed Explanation
This page is the bridge between discovery and commitment. It displays:
- **Rich Content**: Course descriptions pulled from the database and sanitized by the `SafeHtmlRenderer`.
- **Learning Points**: A checklist of "What you'll learn" items.
- **Curriculum**: An interactive accordion showing exactly what is inside every chapter.
- **Sticky Actions**: A persistent card on the right that follows the user as they scroll, providing an "Enroll" or "Go to Course" button.

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1-12** | `import ... from '...'` | **Imports**: Core hooks, API services, and the specialized `SafeHtmlRenderer`. |
| **15** | `const { slug } = useParams();` | **URL**: Extracts the course identifier directly from the browser's address bar. |
| **17** | `useSyncExternalStore(...)` | **State**: Subscribe to `EnrollmentStore` for global enrollment tracking without a context provider. |
| **19** | `enrolled.some(...)` | **Logic**: Derives `isEnrolled` status by searching for the current ID in the global store. |
| **30-33** | `Promise.all([ ... ])` | **Performance**: Executes two high-latency API calls in parallel (Metadata + Structure). |
| **58-82** | `handleEnroll = () => { ... }` | **UX**: Orchestrates the enrollment logic and displays a success toast to the user. |
| **65-70** | `structure?.chapters?.reduce(...)` | **Logic**: Fallback calculation for total lesson count by summing across all chapters. |
| **105-107** | `<SafeHtmlRenderer ... />` | **Security**: High-impact sanitizer that prevents XSS by filtering out dangerous HTML tags. |
| **146** | `<CurriculumAccordion ... />` | **UI**: Renders the complete chapter/lesson tree for student inspection. |
| **151** | `lg:sticky lg:top-24` | **Style**: Ensures the "Enroll" card follows the user as they read long descriptions. |
| **166-194** | `{isEnrolled ? ...}` | **Logic**: Conditional rendering for the CTA button (Go to Course vs Enroll). |

## Why it is used
To convince the student to learn. By showing the requirements, learning points, and exact curriculum, it builds trust and provides clear expectations.

## Interview Questions
1. **Explain the `lg:sticky` behavior on the enrollment card.**
   *Answer: User Experience (UX). As the user reads a long course description, the "Enroll" button stays visible on the right side of the screen. This ensures they don't have to scroll all the way back up or down to find the call to action.*
2. **Why do we use `SafeHtmlRenderer` for the description?**
   *Answer: Security (XSS Prevention). Course descriptions are created by admins using a rich text editor. To prevent malicious code from being injected into the page, the renderer sanitizes the HTML, only allowing safe tags and attributes.*
3. **How does the page calculate the total number of lessons?**
   *Answer: Data Fallback. It first checks if the main course record has a `lessons` count. If not, it uses a `.reduce` function to traverse the `chapters` array and sum up the lengths of all individual lesson lists. This ensures the UI is always accurate regardless of how the backend sent the data.*
  
