# Documentation: AuthService.cs

## Overview
`AuthService.cs` contains the core identity logic for LearnSphere, including registration and login.

## Detailed Explanation
This service acts as an orchestrator. It manages the creation of both a `User` (identity) and a `Student` (profile) during registration. It uses the `PasswordHasher` from ASP.NET Core Identity to securely store and verify passwords, ensuring that plain-text passwords never touch the database.

## Line-by-Line Explanation: IAuthService.cs

| Line(s) | Explanation |
| :--- | :--- |
| **5** | Defines the interface for the identity and authentication layer. |
| **7-8** | **Contract Signatures**: Defines the `RegisterAsync` and `LoginAsync` methods, returning tokens and user profiles on success. |

## Line-by-Line Explanation: AuthService.cs

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **9** | `public class AuthService ...` | **Service**: High-level orchestrator for user identity and security logic. |
| **11-13** | `private readonly ...` | **Deps**: Injected dependencies for Users, Students, and JWT generation. |
| **14** | `PasswordHasher<User> _hasher = new();` | **Security**: Cryptographic tool for one-way password hashing (PBKDF2). |
| **25** | `req.Email.Trim().ToLower();` | **UX**: Normalizes user input to prevent duplicate accounts via casing. |
| **27** | `_users.EmailExistsAsync(email, ct)` | **Guard**: Queries the database to prevent duplicate identity registration. |
| **30-37** | `new User { ... }` | **Data**: Mapping request DTO to the persistent User model with safe defaults. |
| **39** | `_hasher.HashPassword(...)` | **Security**: Transforms the raw password into a secure hash digest. |
| **41** | `await _users.AddAsync(user, ct);` | **Async**: Persists the new identity record to the database. |
| **43-48** | `new Student { ... }` | **Data**: Automatically scaffolds a matching learning profile for the new user. |
| **50** | `await _students.AddAsync(student, ct);` | **Async**: Persists the learning profile to the database. |
| **52** | `await _users.SaveChangesAsync(ct);` | **Async**: Atomically commits both User and Student records (Unit of Work). |
| **56** | `_jwt.CreateToken(user)` | **Security**: Generates a signed JWT session key using custom claims. |
| **67** | `await _users.GetByEmailAsync(...)` | **Logic**: Locates the identity record during the authentication challenge. |
| **71** | `if (user.Status != "active")` | **Guard**: Denies entry if the account has been disabled by an administrator. |
| **74** | `_hasher.VerifyHashedPassword(...)` | **Security**: Securely compares the provided text against the stored cryptographic hash. |
| **80** | `_jwt.CreateToken(user)` | **Security**: Issues a new session token upon successful verification. |

## Program Flow
### Registration:
1. **Uniqueness**: Checks `_users.EmailExistsAsync`.
2. **Identity**: Creates a `User` entity and hashes the password.
3. **Profile**: Automatically creates a linked `Student` entity.
4. **Token**: Generates a JWT via `JwtTokenService`.
### Login:
1. **Retrieval**: Finds the user by email.
2. **Verification**: Compares the provided password against the stored hash.
3. **Status Check**: Denies entry if the account is "inactive".

## Connections
- **Depends on `IUserRepository` & `IStudentRepository`**.
- **Depends on `IJwtTokenService`** for creating the session key.

## Why it is used
It centralizes all security-related logic. By handling both User and Student creation in one place, it ensures "Data Consistency"—you can't have a user without a student profile.

## Interview Questions
1. **Explain why we hash passwords instead of encrypting them.**
   *Answer: Hashing is a "One-Way" process. Once a password is hashed, it cannot be turned back into the original text. Even if a hacker steals the database, they cannot see the users' passwords. Encryption, on the other hand, is two-way and requires a key that could be stolen.*
2. **Why does registration create both a User and a Student?**
   *Answer: To separate authentication (email/password) from the learning profile (grades/guardian info). This architecture allows the same User to potentially act as an Instructor or Admin in the future without breaking their data structure.*
3. **What happens if `user.Status` is not "active"?**
   *Answer: The login method throws an `UnauthorizedAccessException`, which the controller then turns into a "403 Forbidden" or "401 Unauthorized" response.*
4. **Why do we use an Interface (`IAuthService`) instead of just the class?**
   *Answer: Dependency Injection and Testing. By using an interface, we can easily swap out the actual hashing/registration logic for a "Mock" implementation during unit testing without changing the controller's code.*
5. **What is the benefit of `Async` methods in a service?**
   *Answer: Scalability. Async methods allow the server to handle other requests while waiting for the database to respond, preventing "Thread Starvation" during heavy traffic.*
