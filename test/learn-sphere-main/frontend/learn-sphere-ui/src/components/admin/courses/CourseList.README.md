# Documentation: CourseList.jsx (Admin)

## Overview
`CourseList.jsx` is a layout component that displays a responsive grid of courses for administrators.

## Detailed Explanation
It acts as a simple container. It takes an array of `courses` and maps them into `CourseCard` components. It handles the "Just Created" highlight logic, ensuring that if an admin creates a new course, that specific card is scrolled into view or has a specific visual focus.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Imports the `CourseCard` component for child rendering. |
| **3-11** | **Props**: Destructures the list of courses, state management refs, and CRUD callbacks. |
| **13** | **Responsive Grid**: Uses Tailwind to define 1 column on mobile, scaling up to 4 columns on 2xl screens. |
| **14** | **Iterator**: Loops through the `courses` array using standard `map()`. |
| **15-24** | **Child Component**: Renders a `CourseCard` for each course, passing down all necessary data and methods. |
| **17** | **Ref Mapping**: Conditionally attaches the `newCardRef` only if the current ID matches the `justCreatedId`. |

## Program Flow
1. **Mapping**: Iterates through the `courses` array.
2. **Prop Delegation**: Passes down callbacks like `onEdit`, `onDelete`, and `onViewContent` to each individual card.
3. **Ref Handling**: If a `course.id` matches the `justCreatedId`, it assigns a React `ref` to that card so the parent can manage focus.

## Why it is used
To provide a clean, organized view of the platform's catalog. Its CSS Grid configuration ensures the layout looks perfect on everything from small tablets to wide desktop monitors.

## Interview Questions
1. **Explain the `grid-cols-1 md:grid-cols-2 lg:grid-cols-3` classes.**
   *Answer: This is Tailwind's "Mobile First" responsive design. On small screens, there is 1 column. On medium screens (tablets), it switches to 2. On large screens (desktops), it moves to 3. This ensures the cards don't get too squashed or too wide.*
2. **What is the purpose of the `key={course.id}` prop?**
   *Answer: React Performance. The `key` helps React's reconciliation algorithm identify which items in the list have changed, been added, or been removed. Without a unique key, re-rendering a long list becomes very slow.*
