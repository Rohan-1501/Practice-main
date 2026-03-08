# Documentation: Sidebar.jsx (Student Dashboard)

## Overview
`Sidebar.jsx` is a high-performance, animated navigation bar for students.

## Detailed Explanation
This component utilizes a "Hover-Expand" pattern. When idle, it occupies a slim 16px bar showing only icons. When the user hovers their mouse over it, it smoothly transitions to a full 64px width, revealing text labels and sub-menus. It also features a "Notification Badge" system that displays the count of enrolled courses.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Basic imports for React state and navigation routing. |
| **11** | **Props**: Receives `enrolledCount` to display on the courses badge. |
| **13-14** | **State**: Tracks whether the sidebar is `expanded` (hovered) and if the course submenu is open. |
| **18-23** | **Styler Logic**: Helpers for applying Tailwind active/inactive link classes. |
| **26-35** | `Icon`: A reusable functional component that renders any SVG path efficiently. |
| **37-45** | `ICONS`: A dictionary of raw SVG paths for courses, quizzes, and navigation. |
| **51-62** | `Label`: A sub-component that handles the "Fade-In" transition of text when expanding. |
| **65-81** | `CountBadge`: Renders a circular blue notification bubble for course counts. |
| **85-92** | **Container**: The root `div`; uses standard CSS transitions to animate the `width`. |
| **93-94** | **Hover Listeners**: Toggles the expansion state when the mouse enters or leaves. |
| **101-118** | **Sidebar Header**: Displays the app logo (LearnSphere) with expansion animation. |
| **121-186** | **Nav Loop**: Renders the hierarchical navigation items (Courses & Help Center). |
| **128-153** | **Course Parent**: A toggleable button that expands the submenu. |
| **156-175** | **Submenu**: Renders individual links for Enrolled and Completed courses. |

## Program Flow
1. **Hover State**: Uses `onMouseEnter` and `onMouseLeave` to toggle the `expanded` boolean.
2. **Conditional Rendering**: The `Label` and `Chevron` sub-components use CSS opacity and transform transitions to "Fade In" when the sidebar grows.
3. **Submenu Logic**: The "Courses" section can be toggled open/closed independently of the hover state, allowing the user to manage their screen space effectively.

## Why it is used
To maximize the "Learning Area". By staying collapsed during active study, the sidebar doesn't distract the student. By expanding on hover, it provides easy access to navigation without requiring a separate menu button.

## Interview Questions
1. **How is the width transition animated?**
   *Answer: By using Tailwind's `transition-all duration-200` class on the container and conditionally applying `w-16` or `w-64`. CSS transitions automatically interpolate the width change over 200 milliseconds.*
2. **Why do we use SVGs instead of a library like FontAwesome?**
   *Answer: Performance and customization. Using raw SVG paths (defined in the `ICONS` object) allows us to change colors and sizes instantly without loading external font files or heavy icon packages.*
3. **Explain the purpose of the `aria-label` and `role="complementary"` attributes.**
   *Answer: Accessibility (a11y). These attributes help screen readers identify the sidebar as a navigation region, ensuring that visually impaired students can still navigate the platform smoothly.*
