# Documentation: NotificationsController.cs

## Overview
`NotificationsController.cs` is the messaging engine of LearnSphere.

## Detailed Explanation
Interestingly, this controller uses a `static` in-memory list (`notifications`) rather than a database table in this version. This makes it extremely fast but means notifications are lost if the server restarts.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-7** | Imports for Security claims, Authorization, and MVC. |
| **9-10** | Namespace definition for the notifications module. |
| **11-13** | Class marked as an `[ApiController]` with the `[Authorize]` attribute, ensuring only logged-in users access messages. |
| **17** | **Memory Store**: Defines a `static` list that lives as long as the application process runs. |
| **20-22** | `GetNotifications`: Public endpoint to fetch unread messages. |
| **23-26** | Retrieves the User ID from the JWT token for security. |
| **29-32** | **Filter Logic**: Queries the in-memory list for messages belonging to the user that are **not marked read** (`!ReadAt.HasValue`). |
| **33-43** | Maps the internal `Notification` objects to a `NotificationDto` list. |
| **50-64** | `MarkNotificationRead`: Finds a specific notification by ID and **removes it** from the static list to free up memory. |
| **68-78** | `MarkAllRead`: Wipes every notification associated with the calling user. |
| **81-94** | `AddNotificationForUser`: A static helper that other parts of the system (like `CoursesController`) call to push new alerts. |
| **96-103** | `HasNotification`: Checks if a specific type/course alert already exists for a user (prevents spam). |
| **105-108** | `RemoveNotificationsForCourse`: A cleanup helper used when a course is deleted to remove dangling alerts. |
| **112-121** | `GetUnreadCount`: Quickly counts unread items without fetching the full message objects. |

## Program Flow
1. **Broadcast**: Other controllers call `AddNotificationForUser` to add a message to the static list.
2. **Fetch**: The frontend calls the `GET` endpoint to retrieve unread messages for the logged-in user.
3. **Dismiss**: When a user marks a message as "Read", it is physically removed from the static list to save memory.

## Why it is used
It provides a "Live" feel to the platform. Students see red badges and alerts whenever something happens, keeping them engaged.

## HTTP GET Methods: Detailed Explanation

### 1. `GetNotifications()`
- **Endpoint**: `GET /api/Notifications`
- **Working**: Fetches the unread notifications for the currently logged-in user. It filters the in-memory store by `RecipientUserId` and sorts by `CreatedAt` to ensure the most recent alerts are seen first.

### 2. `GetUnreadCount()`
- **Endpoint**: `GET /api/Notifications/unread-count`
- **Working**: A lightweight method that returns only the integer count of unread notifications. This is called frequently by the Navbar to update the "Bell" icon badge.

## HTTP POST Methods: Detailed Explanation

### 1. `MarkNotificationRead(int id)`
- **Endpoint**: `POST /api/Notifications/{id}/read`
- **Working**: Finds a specific notification by its ID and recipient. Instead of toggling a boolean, it **removes** the record from the static list to keep the memory footprint low.

### 2. `MarkAllRead()`
- **Endpoint**: `POST /api/Notifications/read-all`
- **Working**: Clears the entire list of notifications for the current user in one go. Useful for students who want to "Clear the clutter" from their dashboard once they've seen their alerts.

## Interview Questions
1. **What are the pros and cons of using a `static List<Notification>`?**
   *Answer: Pros: Extremely fast performance (no DB overhead). Cons: Data is not persistent (lost on server reboot) and it doesn't scale well horizontally (multiple servers won't see the same list).*
2. **How would you upgrade this to be persistent?**
   *Answer: You would replace the `static List` with an `INotificationRepository` that points to a SQL or NoSQL database table.*
3. **Why does the `MarkNotificationRead` method remove the record instead of just updating a boolean?**
   *Answer: In this specific implementation, it's a memory management strategy. Since it's in-memory, removing "Old" data prevents the server from running out of RAM over time.*
