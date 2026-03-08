# Documentation: User.cs

## Overview
`User.cs` is the top-level authentication and profile management model. it represents any registered individual in the LearnSphere system, whether they are a student or an admin.

## Detailed Explanation
The model uses a `Guid` for the unique identifier to ensure global uniqueness. It stores basic security and identification data like email, password hashes, and roles. It also acts as a bridge to the more detailed `StudentProfile`.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **3** | `public class User` | Defines the master identity entity for authentication. |
| **5** | `public Guid Id { ... }` | Globally unique ID (prevents enumeration attacks). |
| **7** | `public string Name { ... }` | User's chosen display name for the profile header. |
| **8** | `public string Email { ... }` | Login identifier (enforced unique in DB). |
| **9** | `public string PasswordHash ...` | Securely salted hash of the user's password. |
| **11** | `public string Role { ... }` | Identity tag: "student" (Default) or "admin". |
| **12** | `public string Status { ... }` | Lifecycle: "active" (Normal) or "inactive" (Locked). |
| **14** | `public DateTime CreatedAt ...` | Timestamp of registration (Normalized to UTC). |
| **16** | `public Student? StudentProfile` | Navigation to the detailed academic/student record. |
| **17** | `}` | End of the top-level User identity model. |

## Program Flow
1. **Registration/Login**: During auth flows, the `UsersController` or `AuthController` interacts with this model to validate credentials.
2. **Authorization**: The `Role` property is used by middleware to restrict access to certain API endpoints (e.g., admin-only routes).
3. **Profile Linking**: If a user is a student, the system navigates through the `StudentProfile` property to access academic details.

## Connections
- **Connected to `Student`**: Via the `StudentProfile` navigation property (One-to-One relationship).
- **Used by `AuthController`**: For generating JWT tokens based on the `Role` and `Email`.
- **Used by `ApplicationDbContext`**: To map to the `Users` table.

## Properties Explanation
- `Id`: Globally Unique Identifier (GUID) for the user.
- `Name`: User's full name for display.
- `Email`: Unique credential used for login.
- `PasswordHash`: Securely stored hashed password (never store plain text!).
- `Role`: Defines permissions (e.g., "student", "admin").
- `Status`: Indicates if the account is "active" or "inactive".
- `CreatedAt`: Timestamp of when the account was created.

## Why it is used
It is the primary entity for identity management. It separates authentication data (User) from academic/personal data (Student) to keep the system modular and secure.

## Interview Questions
1. **Why use `Guid` instead of `int` for the User Id?**
   *Answer: GUIDs are better for distributed systems as they can be generated client-side or across multiple servers without risk of collision, and they don't reveal information about the total number of users in the system.*
2. **What is the security risk of storing `PasswordHash` instead of plain passwords?**
   *Answer: Actually, it's the opposite—storing plain text is the risk. Storing a hash (with salt) means that even if the database is leaked, attackers cannot immediately see user passwords.*
3. **What does `public Student? StudentProfile { get; set; }` represent in Entity Framework?**
   *Answer: It's a navigation property representing a one-to-one relationship. The `?` indicates it's optional, as not all users (like admins) might have a student profile.*
