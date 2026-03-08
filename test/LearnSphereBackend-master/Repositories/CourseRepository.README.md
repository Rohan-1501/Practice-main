# Documentation: CourseRepository.cs

## Overview
`CourseRepository.cs` provides the basic CRUD operations for the platform's courses.

## Detailed Explanation
It handles the creation, retrieval, updating, and deletion of course records. It includes a specific `UpdateAsync` method that manually maps properties from a DTO-like object to the existing database entity, ensuring that the `UpdatedAt` timestamp is always refreshed.

## Line-by-Line Explanation: ICourseRepository.cs

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Imports the `Course` model. |
| **2** | Defines the namespace for repository interfaces. |
| **4** | Defines the `ICourseRepository` interface to provide a contract for course operations. |
| **6-10** | **Contract Methods**: Declares asynchronous signatures for Get, Add, Update, and Remove actions. |

## Line-by-Line Explanation: CourseRepository.cs

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **8** | `public class CourseRepository ...` | **Class**: Implements the course data contract for SQL interaction. |
| **10** | `private readonly AppDbContext _db;` | **Data**: Injected database context for physical I/O. |
| **11** | `public CourseRepository(...)` | **Constructor**: Receives the DbContext from the DI container. |
| **15** | `_db.Courses.Add(course);` | **Logic**: Queues a new course for insertion into the Change Tracker. |
| **16** | `await _db.SaveChangesAsync();` | **Async**: Commits the tracker's changes to the physical SQL table. |
| **22** | `.AsNoTracking()...` | **Optim**: Fetches courses without tracking overhead (read-only performance). |
| **27** | `_db.Courses.FindAsync(id);` | **Async**: Uses Primary Key lookup for the fastest possible record retrieval. |
| **32** | `await _db.Courses.FindAsync(id);` | **Logic**: Locates the victim record before attempting deletion. |
| **34** | `_db.Courses.Remove(item);` | **Logic**: Marks the entity for deletion in the tracking context. |
| **41** | `await _db.Courses.FindAsync(id);` | **Logic**: Retrieves the current database state for specialized field mapping. |
| **44-53** | `existing.Title = course.Title; ...` | **Logic**: Explicitly copies only modified fields to prevent data corruption. |
| **54** | `existing.UpdatedAt = DateTime.UtcNow;` | **Audit**: Automatically logs the modification timestamp in universal time. |
| **56** | `_db.Courses.Update(existing);` | **Logic**: Re-attaches or ensures the entity is marked as modified. |
| **57** | `await _db.SaveChangesAsync();` | **Async**: Finalizes the update in the database. |

## Program Flow
1. **List**: Fetches all courses, ordered by the most recently created.
2. **Update**: Finds the existing course, updates specific fields (Slug, Summary, etc.), and saves changes.

## Interview Questions
1. **Why is the `OrderByDescending` important in `GetAllAsync`?**
   *Answer: In a learning platform, users expect to see the "Newest" courses at the top of the search results or landing page.*
2. **Explain the manual mapping approach in the `UpdateAsync` method.**
   *Answer: Instead of replacing the entire object, the repository picks specific fields to update. This is safer as it prevents a user from accidentally overwriting internal fields like the original `CreatedAt` date.*
3. **What happens if `RemoveAsync` is called for an ID that doesn't exist?**
   *Answer: The method returns `false`, allowing the controller to send a specific "404 Not Found" response back to the user.*
4. **Why do we use an Interface (`ICourseRepository`) instead of just the class?**
   *Answer: Dependency Injection and Testing. By using an interface, we can easily swap out the SQL implementation for a "Mock" implementation during unit testing without changing the controller's code.*
5. **What is the benefit of `Async` methods in a repository?**
   *Answer: Scalability. Async methods allow the server to handle other requests while waiting for the database to respond, preventing "Thread Starvation" during heavy traffic.*
