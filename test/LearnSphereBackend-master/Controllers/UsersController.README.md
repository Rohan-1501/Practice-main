# Documentation: UsersController.cs

## Overview
`UsersController.cs` provides administrative endpoints for managing the user database.

## Detailed Explanation
This controller is protected by the `[Authorize]` attribute. It includes logic to manually verify that the requesting user has the "admin" role before allowing them to see all users or update an account status.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-4** | Imports for Security Authorization, MVC, DTOs, and Repository interfaces. |
| **6** | Namespace definition for the user-management controller. |
| **8-10** | Class marked as an `[ApiController]` with base route `api/users`. |
| **12-17** | **Constructor**: Injects the `IUserRepository` via Dependency Injection. |
| **24** | `GetAll`: Admin-only endpoint to list every user account. |
| **27-31** | **Role Check**: Manually verifies that the user is authenticated and has the "admin" role before proceeding. |
| **33** | Fetches the full list of users from the repository. |
| **34-58** | **Mapping**: Converts each `User` model into a `UserDto`. Note how it nestedly maps the `StudentProfile` if it exists. |
| **60** | Returns the list of DTOs with a `200 OK` status. |
| **68** | `UpdateStatus`: Admin endpoint to activate/deactivate a user. |
| **71-75** | Redundant role check to ensure maximum security for destructive actions. |
| **78-80** | Searches for the target user by their unique `Guid` ID. Returns `404` if not found. |
| **82-83** | **Input Validation**: Ensures the requested status is either "active" or "inactive". |
| **85-86** | Updates the model property and persists the change to the database. |

## Program Flow
1. **Authorization Check**: Every method first verifies `User.IsInRole("admin")`.
2. **Data Mapping**: Fetches raw `User` entities from the repository and maps them into `UserDto` objects, including nested student profile data if available.
3. **Status Update**: Allows an admin to toggle a user between "active" and "inactive".

## Connections
- **Depends on `IUserRepository`**: For data access.
- **Uses `UserDto`**: To return structured data to the admin panel.

## Why it is used
It gives platform administrators control over the user base. It acts as the backend for the "User Management" section of the Admin Dashboard.

## HTTP GET Methods: Detailed Explanation

### 1. `GetAll()`
- **Endpoint**: `GET /api/users`
- **Access**: Admin Only
- **Working**: An Admin-only endpoint that returns a list of every user in the database, including their role (Student/Admin) and account status. It also "Nests" the student profiles within each user record to provide a complete view of the user.

## HTTP POST, PUT & DELETE Methods: Detailed Explanation

### 1. `UpdateStatus(Guid id)`
- **Endpoint**: `PUT /api/users/{id}/status`
- **Access**: Admin Only
- **Working**: Allows admins to deactivate or reactivate an account. This is useful for suspending users who violate terms or handling account deletions by setting them to "inactive".

## Interview Questions
1. **How is security enforced in this controller?**
   *Answer: In two ways: first, the `[Authorize]` attribute ensures the user is logged in. Second, an explicit code check `User.IsInRole("admin")` ensures the user has permission to perform administrative actions.*
2. **What is the risk of returning the `u.StudentProfile` within the `UserDto`?**
   *Answer: It could lead to a "Select N+1" performance issue if the repository doesn't eagerly load those profiles. The documentation should clarify that the repository should use `.Include(u => u.StudentProfile)`.*
3. **Why return `403 Forbidden` instead of `401 Unauthorized` for a non-admin user?**
   *Answer: 401 means the server doesn't know who you are. 403 means the server knows who you are, but you explicitly do not have permission for this specific action.*
