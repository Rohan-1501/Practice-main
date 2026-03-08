# Documentation: NotificationRepository.cs

## Overview
`NotificationRepository.cs` is currently a placeholder class.

## Detailed Explanation
In the current architecture of LearnSphere, notifications are managed via a `static` in-memory list inside the `NotificationsController`. This repository exists to fulfill the project structure but currently contains no logic.

## Interview Questions
1. **How would you implement the methods in this repository if the project moved to a persistent database?**
   *Answer: You would inject the `AppDbContext`, and implement methods like `AddAsync(Notification n)`, `GetUnreadByUserAsync(Guid userId)`, and `MarkAllAsReadAsync(Guid userId)` using EF Core queries.*
