# Documentation: CourseCard.jsx (Admin)

## Overview
`CourseCard.jsx` provides a summary of a single course within the admin management view.

## Detailed Explanation
Unlike the student-facing card, the Admin Card focuses on management metadata. It shows the `status` (Draft, Published, Archived) with color-coded badges and includes a `CourseCardMenu` flyout for quick actions. It uses `React.forwardRef` to allow the parent `CourseList` to focus on the card if it was recently created.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **6-9** | **Ref Forwarding**: Uses `React.forwardRef` to pass the focus target to the root `<article>`. |
| **11-13** | **Container**: Sets up the semantic card wrapper with click-to-preview logic. |
| **14-22** | **Dynamic Styling**: Applies borders, backgrounds, and highlighting if the card is "selected". |
| **25-34** | **Thumbnail**: Renders the image or a CSS gradient fallback if the URL is missing. |
| **37-41** | **Header Section**: Displays the course title and the management action menu. |
| **43-53** | **Status Pill**: Conditional classes for coloring (Green/Yellow/Gray) based on state. |
| **55-57** | **Description**: Truncated summary text using `line-clamp-2`. |
| **59-77** | **Meta Info**: Displays level, duration, and the first two categories as comma-separated tags. |

## Program Flow
1. **Thumbnail Rendering**: Uses a CSS background image for the thumbnail. If no image exists, it shows a beautiful purple-indigo gradient.
2. **Status Badge**: Uses conditional logic to apply different background colors (Green for published, Yellow for draft) based on the course's current state.
3. **Interaction**: Clicking the card triggers the `onViewContent` callback, taking the admin into the curriculum manager.

## Why it is used
It acts as the "Entry Point" for course editing. It provides just enough information (Title, Level, Duration) for an admin to identify a course at a glance.

## Interview Questions
1. **Why is `React.forwardRef` used here?**
   *Answer: To allow parent components to access the underlying DOM element of the card. This is used in `CourseList` to scroll a newly created course card into view automatically.*
2. **Explain the logic in the `style` prop of the thumbnail div.**
   *Answer: It is a fallback system. It first tries to use the `course.thumbnail` URL. If that is empty, it applies a `linear-gradient`. This ensures that even "New" courses without a thumbnail still look premium in the UI.*
3. **How does the description truncation work in this file?**
   *Answer: It uses the Tailwind `line-clamp-2` class. This uses CSS to limit the text to exactly two lines and add "..." at the end if it overflows, keeping all cards the same height.*
