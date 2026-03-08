# Documentation: LiveSessionsController.cs

## Overview
`LiveSessionsController.cs` manages scheduled, synchronous learning events.

## Detailed Explanation
The controller allows admins to schedule live classes (with optional video/thumbnail uploads) and allows students to "Join" these sessions. It uses Indian Standard Time (IST) formatting for its notification system to ensure local consistency for the platform's primary users.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-8** | Imports for Security (Claims), Authorization, MVC, EF Core, and internal Application interfaces. |
| **10** | Namespace for organizing live session logic. |
| **16-18** | Private fields for the Database context, File Upload service, and Student repository. |
| **20-25** | **Constructor**: Initializes the private fields using Dependency Injection. |
| **29-35** | `GetAll`: Retrieves all live sessions from the DB, ordered by the most recent start time. |
| **38-44** | `GetById`: Finds a specific session by its primary key (ID). |
| **49** | `JoinSession`: Student-only endpoint to record attendance. |
| **51-53** | **Auth Check**: Retrieves the student's unique ID from the JWT token claims. |
| **55-59** | Fetches the student profile and the target session to ensure they exist. |
| **61-65** | **Time Guard**: Checks if the current time is actually within the session's active window. |
| **70-79** | Adds a new attendance record if one doesn't already exist for this student/session. |
| **87** | `Create`: Admin endpoint to schedule a new class. Uses `[FromForm]` for file uploads. |
| **90-99** | **Validation**: Ensures start time is in the future and end time is after start time. |
| **104-116** | Processes optional Video and Thumbnail files using the `_fileUpload` service. |
| **118-128** | Instantiates a new `LiveSession` model with the provided data. |
| **136-155** | **Global Notification**: Loops through all students to notify them of the new session in IST time. |
| **162** | Returns `201 Created` with the location of the new session. |
| **168-216** | `Update`: Modifies an existing session. Intelligently updates files only if new ones are provided. |
| **221-234** | `Delete`: Removes the session from the DB and **deletes the physical files** from the server's disk. |
| **240-245** | `GetIstZone`: A cross-platform helper to find the correct IST timezone handle for both Windows and Linux. |
| **248-261** | **DTOs**: Defines the structure for incoming create/update requests. |

## Program Flow
1. **Scheduling**: Admin creates a session with `StartTime` and `EndTime`.
2. **Notification**: Upon creation, the controller automatically blasts a notification to ALL registered students.
3. **Joining**: Students can click "Join". The controller verifies that the current time is within the session's window before recording `LiveSessionAttendance`.

## Why it is used
It enables "Real-Time" interaction, making the platform more than just a library of pre-recorded videos.

## HTTP GET Methods: Detailed Explanation

### 1. `GetAll()`
- **Endpoint**: `GET /api/LiveSessions`
- **Working**: This method returns a list of all live sessions, sorted by their `StartTime` (newest first). It is used to build the "Schedule" view for students and admins.

### 2. `GetById(int id)`
- **Endpoint**: `GET /api/LiveSessions/{id}`
- **Working**: Fetches the full metadata for a specific session. This is typically called just before a student enters the "Live Player" room.

## HTTP POST, PUT & DELETE Methods: Detailed Explanation

### 1. `Create()`
- **Endpoint**: `POST /api/LiveSessions`
- **Access**: Admin Only
- **Working**: Heavily uses `[FromForm]` to handle mixed data (Title, Times) and binary files (Video/Thumbnail). It uploads the files to disk, saves the session to the DB, and then triggers a **New Live Session** notification to all students.

### 2. `JoinSession(int id)`
- **Endpoint**: `POST /api/LiveSessions/{id}/join`
- **Working**: Acts as a "Digital Attendance" sheet. It first verifies that the session is currently active (Time check). If active, it creates a `LiveSessionAttendance` record for the student.

### 3. `Update(int id)`
- **Endpoint**: `PUT /api/LiveSessions/{id}`
- **Access**: Admin Only
- **Working**: Allows admins to modify scheduled times or replace the media files. It intelligently handles optional file updates—if no new file is provided, it keeps the old one.

### 4. `Delete(int id)`
- **Endpoint**: `DELETE /api/LiveSessions/{id}`
- **Access**: Admin Only
- **Working**: Completely removes the session. Importantly, it also calls `FileUploadService.DeleteFileAsync` to physically remove the video and thumbnail files from the server's disk to save space.

## Interview Questions
1. **How do you handle timezones in this controller?**
   *Answer: The database stores all dates in UTC (Universal Time Coordinated) for absolute consistency. The controller only converts to IST when generating the human-readable notification message.*
2. **What happens if a student tries to join a session that hasn't started yet?**
   *Answer: The `JoinSession` method checks the current time against `session.StartTime`. If it's too early, it returns a 200 OK with a `joined: false` flag and a helpful message.*
3. **Why use `[FromForm]` for the Create/Update methods?**
   *Answer: Because these endpoints accept binary files (VideoFile, ThumbnailFile). JSON cannot natively transmit binary files, so `multipart/form-data` (form) is required.*
