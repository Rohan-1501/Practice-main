# Documentation: Notification.cs

## Overview
`Notification.cs` represents an alert or message sent to a user within the LearnSphere platform.

## Detailed Explanation
Notifications are used to inform users about important events, such as a new course being added, a live session starting, or an assessment result being available. It tracks whether the notification has been read.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Defines the `Notification` class for in-app alerts. |
| **3** | `Id`: Primary key for the notification. |
| **4** | `RecipientUserId`: GUID of the user receiving the alert. |
| **5-6** | **Payload**: The Title and Message content of the notification. |
| **7** | `CreatedAt`: When the alert was generated. |
| **8** | `ReadAt`: Timestamp for when the user dismissed or opened the alert. |
| **9** | `CourseId`: Optional context linking the alert to a specific course. |
| **10** | `Type`: Category of alert (e.g., "Info", "Alert", "CourseUpdate"). |

## Program Flow
1. **Trigger**: An event occurs (e.g., a new course is published).
2. **Creation**: The `NotificationsController` creates a `Notification` record for each target user.
3. **Retrieval**: When a user logs in, the frontend fetches all unread notifications.
4. **Interaction**: When the user clicks a notification, the `ReadAt` timestamp is updated.

## Connections
- **Connected to `User`**: Linked via `RecipientUserId`.
- **Connected to `Course`**: Optional link to a specific course for "Contextual" notifications.

## Properties Explanation
- `RecipientUserId`: The GUID of the user who should receive the message.
- `Title` & `Message`: The content of the alert.
- `ReadAt`: Null if unread; stores the time when the user viewed the notification.

## Why it is used
It improves user engagement and ensures that students don't miss important dates or updates to their courses.

## Interview Questions
1. **How do you query for "Unread" notifications?**
   *Answer: By filtering records where `RecipientUserId` matches the current user and `ReadAt` is null.*
2. **What is the purpose of the `CourseId` in a notification?**
   *Answer: It allows the frontend to provide a "Deep Link". For example, clicking "New Course Added" can immediately redirect the user to that specific course page.*
3. **Why use `int?` for `CourseId` instead of a regular `int`?**
   *Answer: Because not all notifications are about courses. Some might be system-wide alerts or personal messages.*
