# Documentation: UserRepository.cs

## Overview
`UserRepository.cs` encapsulates all database operations related to the `User` entity.

## Detailed Explanation
It implements the `IUserRepository` interface, providing methods for checking email existence, retrieving a user by email, and fetching all users with their student profiles. It uses `AppDbContext` and Entity Framework Core for data retrieval.

## Line-by-Line Explanation: IUserRepository.cs

| Line(s) | Explanation |
| :--- | :--- |
| **5** | Defines the interface for user account management. |
| **7-8** | `EmailExistsAsync` / `GetByEmailAsync`: Essential for authentication and registration workflows. |
| **9** | `GetAllAsync`: Fetches all user records (admin access). |
| **10** | `AddAsync`: Pushes a new user entity to the tracker. |

## Line-by-Line Explanation: UserRepository.cs

| Line(s) | Explanation |
| :--- | :--- |
| **1-4** | Standard imports for Data Context, Models, and the Interface contract. |
| **8** | Class declaration for the physical `UserRepository` implementation. |
| **12** | **Constructor**: Expression-bodied constructor that assigns the local DB context. |
| **14-15** | `EmailExistsAsync`: Uses the efficient `AnyAsync` to return a boolean if an email is taken. |
| **17-18** | `GetByEmailAsync`: Performs a lookup for a specific user object based on their email string. |
| **20-21** | `GetAllAsync`: Fetches everyone and **eager-loads** their `StudentProfile`. Note the `.ContinueWith` used for mapping. |
| **23-24** | `AddAsync`: Inserts a new user into the database asynchronously. |
| **26-27** | `SaveChangesAsync`: Commits the pending user changes to the SQL server. |

## Program Flow
1. **Query**: The repository receives a request (e.g., `GetByEmailAsync`).
2. **EF Core Execution**: It translates the C# LINQ query into SQL.
3. **Eager Loading**: For `GetAllAsync`, it uses `.Include(u => u.StudentProfile)` to ensure that student details are fetched in the same database trip.

## Why it is used
It isolates the data access logic from the rest of the application. If the database schema changes, you only need to update this file, not the controllers or services.

## Interview Questions
1. **Explain the benefits of the Repository Pattern.**
   *Answer: It promotes "Separation of Concerns". It makes the code more testable (by allowing you to mock the interface) and keeps the controllers clean of database-specific logic (like `.Include()` or `.AsNoTracking()`).*
2. **What does `.AsNoTracking()` do in a repository?**
   *Answer: It tells EF Core not to track the returned entities in its internal cache. This is a performance optimization for "Read-Only" operations, as it reduces memory usage and processing time.*
3. **Why use `CancellationToken ct` in every method?**
   *Answer: To support asynchronous programming best practices. It allows the database operation to be aborted if the parent request is cancelled, preventing "Zombie" queries from hanging around.*
4. **Why do we use an Interface (`IUserRepository`) instead of just the class?**
   *Answer: Dependency Injection and Testing. By using an interface, we can easily swap out the SQL implementation for a "Mock" implementation during unit testing without changing the code that uses it.*
5. **What is the benefit of `Async` methods in a repository?**
   *Answer: Scalability. Async methods allow the server to handle other requests while waiting for the database to respond, preventing "Thread Starvation" during heavy traffic.*
