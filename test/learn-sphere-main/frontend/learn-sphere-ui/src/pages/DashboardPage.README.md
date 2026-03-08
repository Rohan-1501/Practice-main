# Documentation: DashboardPage.jsx

## Overview
`DashboardPage.jsx` is the primary hub for students to access their courses and live sessions.

## Detailed Explanation
This page serves as a high-density "Activity Center". It combines data from three distinct sources:
1. **Live Sessions**: Real-time events with countdown/status badges.
2. **Explore Courses**: A discovery grid for all available content.
3. **Enrollment Store**: A real-time external store that tells the page exactly which courses the student already owns.

It also features a "Fluid Sidebar" layout. The page automatically measures the width of the sidebar using a `ResizeObserver` and adjusts the main content's margins, ensuring the UI never looks broken on different screen sizes.

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **5** | `useLayoutEffect` | **Hooks**: Used for measurement to prevent layout shift before the browser paints. |
| **7** | `useSyncExternalStore` | **State**: Reactive subscription to the global `EnrollmentStore`. |
| **32-54** | `useLayoutEffect(() => { ... })` | **UI**: Implements a `ResizeObserver` to dynamically adjust content margins for the sidebar. |
| **58** | `enrolled = useSyncExternalStore(...)` | **Sync**: Connects the UI to the non-React enrollment data source. |
| **63-66** | `Promise.all([ ... ])` | **Async**: Parallelized fetching for Course catalog and Live Sessions. |
| **69-76** | `const normalizeDate = ...` | **Logic**: Critical utility that forces UTC parsing to prevent timezone mismatch. |
| **80-89** | `liveData.map((s) => { ... })` | **Logic**: Real-time status calculation (Live/Upcoming/Ended) against `new Date()`. |
| **140-154** | `formatDateTime` | **UI**: Localizes backend UTC dates into user-friendly strings (e.g., "Mon, Oct 12"). |
| **156-174** | `useMemo(...)` | **Performance**: Caches filtered and sorted lists to prevent lag during rapid search typings. |
| **189** | `<Sidebar />` | **Layout**: Renders the global navigation sidebar within its measured container. |
| **268** | `className="... animate-pulse"` | **UX**: High-impact visual for active sessions to grab user attention. |
| **289-310** | `onClick={async (e) => { ... }}` | **Logic**: Automates participation tracking when joining an active session. |
| **361-368** | `{enrolled.some(...) && (...)}` | **Data**: Conditionally displays "Enrolled" or "Completed" badges on course cards. |

## Why it is used
To provide a personalized starting point. It helps students jump back into their learning or discover new opportunities without feeling overwhelmed.

## Interview Questions
1. **Explain the purpose of `useLayoutEffect` in this page.**
   *Answer: Prevent Layout Shift. Unlike `useEffect`, which runs after the browser paints, `useLayoutEffect` runs before. We use it to measure the sidebar's width so we can set the main content's margin perfectly, ensuring the user never sees the content "jump" into place.*
2. **Why do we use `useSyncExternalStore` here?**
   *Answer: Real-time State Synchronization. The "Enrollment" state lives in a special object outside of React's regular state (in `EnrollmentStore.js`). This hook allows the Dashboard to "Subscribe" to that store so it stays in perfect sync whenever the user enrolls/unenrolls anywhere in the app.*
3. **How does the page handle different timezones for Live Sessions?**
   *Answer: Normalization. The backend sends UTC strings. The `normalizeDate` utility ensures that every string is treated as UTC (by appending 'Z' if missing). JavaScript then automatically converts this to the student's local browser time for display.*
  
