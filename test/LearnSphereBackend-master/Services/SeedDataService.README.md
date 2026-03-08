# Documentation: SeedDataService.cs

## Overview
`SeedDataService.cs` is a utility tool used to initialize the platform with a "Master Admin" account.

## Detailed Explanation
This static service is typically called during the application startup process. It checks if an admin account exists (defaulting to `admin@example.com`). If not, it creates one with a default password. It also serves as an "Enforcer", making sure that even if the admin account already exists, it is correctly set to the "admin" role and "active" status.

## Line-by-Line Explanation: SeedDataService.cs

| Line(s) | Explanation |
| :--- | :--- |
| **1-3** | Imports for Identity hashing, DbContext, and Models. |
| **7** | Defines the class as `static` so it can be called without instantiation during startup. |
| **9** | `SeedAdminUserAsync`: Entry point called in `Program.cs`. |
| **11** | **Scope Management**: Manual creation of a DI scope to fetch the DB context outside a request. |
| **12** | Resolves the `AppDbContext` using the provided service provider. |
| **15** | **Detection**: Queries the user table for the specific hardcoded admin email. |
| **16** | Initializes the hasher for secure credential management. |
| **18** | Branch for "First Time Setup" if the admin doesn't exist. |
| **20-27** | **Account Creation**: Populates the properties and hashes the default password `Instructor@123`. |
| **28** | Adds the new record to the tracking set. |
| **31-38** | Branch for "Enforcement" if the email exists but credentials/roles might be wrong. |
| **33-34** | Hard-resets the role to "admin" and status to "active". |
| **36-37** | Re-hashes the default password to ensure the admin can always regain entry. |
| **40** | Commits all changes (Creation or Update) to the database. |

## Program Flow
1. **Scope Creation**: Since it's a static service, it creates a temporary dependency scope to access the `AppDbContext`.
2. **Detection**: Looks for the email `admin@example.com` in the user table.
3. **Creation/Update**:
   - If missing: Creates the user with the password `Instructor@123`.
   - If present: Updates the password and role to ensure the "Master Key" always works.

## Why it is used
To solve the "Cold Start" problem. When the platform is first deployed to a new server, there are no users. This service ensures that there is at least one admin who can log in and start creating courses.

## Interview Questions
1. **Why is this a `static` class?**
   *Answer: Because it contains a "Utility" method that doesn't hold any state. Making it static makes it easy to call from `Program.cs` without having to register it in the dependency injection container.*
2. **What are the risks of using a hardcoded password like `Instructor@123`?**
   *Answer: It is a major security risk if left in production. This pattern is strictly for "Local Development" or "Staging". In a real production app, you would use an Environment Variable or a one-time setup script.*
3. **Why do we call `serviceProvider.CreateScope()`?**
   *Answer: Because `AppDbContext` is a "Scoped" service (shared per-request). Since application startup isn't a "Request", we have to manually create a scope so that the database connection can be correctly managed and closed.*
