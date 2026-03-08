# Documentation: LiveSession.cs

## Overview
`LiveSession.cs` represents a scheduled real-time interaction between instructors and students, such as a webinar or a live lecture.

## Detailed Explanation
Unlike recorded lessons, live sessions have a strict `StartTime` and `EndTime`. They are associated with a course and provide a `VideoUrl` (usually a link to a streaming platform like Zoom or YouTube Live).

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for schema constraints and mapping. |
| **6** | Defines the `LiveSession` class for synchronous events. |
| **9** | `Id`: Primary key for the session. |
| **12** | `Title`: The name of the event (e.g., "Q&A with Instructor"). |
| **14** | `Description`: brief context or agenda for the live session. |
| **17** | `VideoUrl`: Link to the streaming provider (Zoom, Webex, YouTube). |
| **19** | `ThumbnailUrl`: Preview image for the session card. |
| **22-25** | **Schedule**: Start and End times in UTC for the event window. |
| **27-30** | **Parent Link**: Optional connection to a specific `Course`. |
| **32-33** | **Auditing**: Creation and last update timestamps. |

## Program Flow
1. **Scheduling**: An admin creates a session with a specific time window.
2. **Notification**: The system notifies students enrolled in the associated course about the upcoming session.
3. **Joining**: At the `StartTime`, the session becomes "Live" on the frontend, and the `VideoUrl` is made accessible.

## Connections
- **Connected to `Course`**: Optional link. A session can be course-specific or general.
- **Connected to `LiveSessionAttendance`**: Used to track who actually showed up.

## Properties Explanation
- `VideoUrl`: The link to the live stream.
- `StartTime` / `EndTime`: The scheduled window for the event.
- `CourseId`: Links the session to a specific course's students.

## Why it is used
It facilitates synchronous learning within an otherwise asynchronous platform. It allows for "Event-driven" educational content.

## Interview Questions
1. **How do you handle different timezones with `StartTime`?**
   *Answer: By storing all dates in UTC format in the database (as seen by the `DateTime.UtcNow` default) and converting them to the user's local time on the frontend.*
2. **What validation would you add to this model?**
   *Answer: Ensure that `EndTime` is always greater than `StartTime`.*
3. **What is the purpose of making `CourseId` nullable (`int?`)?**
   *Answer: It allows for "Public" live sessions that aren't tied to any specific course, making the platform more flexible.*
