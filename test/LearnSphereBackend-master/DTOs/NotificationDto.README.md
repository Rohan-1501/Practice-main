# Documentation: NotificationDto.cs

## Overview
`NotificationDto.cs` is the data structure for the notifications visible in the user's "Inbox" or sidebar.

## Detailed Explanation
It maps a `Notification` entity into a format optimized for the UI, including a flag `IsRead` (which is calculated based on whether `ReadAt` is null in the database).

## Properties Explanation
- `Type`: Categorizes the notification (e.g., "LIVE_CLASS").
- `CourseId`: Allows the user to navigate directly to the related course.

## Interview Questions
1. **What is "Deep Linking" in the context of `CourseId`?**
   *Answer: It is the ability to take a user from a notification directly to a specific page on the site, rather than just the home page.*
2. **Why include the `CreatedAt` date?**
   *Answer: So the frontend can show "2 minutes ago" or "Yesterday", which helps the user prioritize which alerts to read first.*
