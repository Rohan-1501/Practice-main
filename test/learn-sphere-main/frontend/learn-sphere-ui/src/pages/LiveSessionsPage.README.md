# Documentation: LiveSessionsPage.jsx

## Overview
`LiveSessionsPage.jsx` is the dedicated catalog for all real-time and recorded learning events.

## Detailed Explanation
While the Dashboard shows a small preview, this page gives students a full view of all scheduled sessions. It uses a high-contrast theme and includes:
- **Status Badges**: Instant visual cues for what is happening "Right Now".
- **Advanced Scheduling**: Displays the exact weekday, time, and duration of every event.
- **Recording Access**: Once a session is "ENDED", the button automatically changes to "View Recording".

## Program Flow
1. **Dynamic State Hydration**: If the user navigated from the Dashboard, the page reuses the data already fetched. If not, it calls `liveSessionApi.getAll()` to get the latest schedule.
2. **Attendance Logic**: When a student clicks "Join", the page first calls the `join` API (to record their attendance for analytics) before taking them to the player.
3. **Automatic Sorting**: Sessions are filtered by status and can be navigated easily.

## Why it is used
To manage "Time-Bound" learning. It helps students plan their week around live lectures and ensures they never miss a session even after it's over.

## Interview Questions
1. **How do you minimize redundant API calls in this page?**
   *Answer: Using React Router's `location.state`. When a student clicks "View all" on the Dashboard, the Dashboard passes the data it already has. This page checks `location.state` first. If the data is there, it skips the network request entirely, making the app feel much faster.*
2. **Explain the `isLive` calculation.**
   *Answer: Boundary Testing. We take the current time and check if it is "Greater than or equal to" the start time AND "Less than or equal to" the end time. If both are true, the session is active.*
3. **What is the benefit of the `weekday: "short"` formatting in `formatDateTime`?**
   *Answer: Readability. Instead of just a date like "12/04", showing "Wed, Dec 4" helps the student quickly identify when they need to be online without checking a calendar.*
  
