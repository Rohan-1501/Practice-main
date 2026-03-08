# Documentation: CourseStructureRepository.cs

## Overview
`CourseStructureRepository.cs` is a multi-class file that contains the repositories for Chapters, Lessons, and Course Content.

## Detailed Explanation
Instead of having three separate files, this project groups the "Child" hierarchies together. 
- **ChapterRepository**: Manages the sections of a course.
- **LessonRepository**: Manages the individual units within a chapter.
- **CourseContentRepository**: Manages the files (video/PDF) within a lesson.

## Line-by-Line Explanation: ICourseStructureRepository.cs

| Line(s) | Explanation |
| :--- | :--- |
| **5-13** | `IChapterRepository`: Defines the standard interface for handling high-level course sections. |
| **15-23** | `ILessonRepository`: Defines the interface for individual learning units. |
| **25-33** | `ICourseContentRepository`: Defines the interface for the actual learning files (Videos, PDFs). |

## Line-by-Line Explanation: CourseStructureRepository.cs

### ChapterRepository
| Line(s) | Explanation |
| :--- | :--- |
| **8** | Class declaration implementing the `IChapterRepository` interface. |
| **12-15** | **Constructor**: Injects `AppDbContext` for data persistence. |
| **19-23** | `GetByCourseIdAsync`: Fetches chapters for a course, eager-loading lessons and sorting by `Order`. |
| **28-31** | `GetByIdAsync`: Retrieves a chapter by ID, including its parent Course and child Lessons for context. |
| **41-42** | `UpdateAsync`: Marks the entity as updated in the context. Uses `Task.CompletedTask` to match interface. |
| **47-51** | `DeleteAsync`: Safely removes a chapter by fetching it first. |

### LessonRepository
| Line(s) | Explanation |
| :--- | :--- |
| **60** | Class declaration for lesson management. |
| **71-75** | `GetByChapterIdAsync`: Pulls lessons for a chapter, including all nested `Contents` (media files). |
| **80-83** | `GetByIdAsync`: Deep fetch of a lesson with its parent Chapter and internal media list. |
| **93-94** | `UpdateAsync`: Syncs lesson state without immediate DB execution. |

### CourseContentRepository
| Line(s) | Explanation |
| :--- | :--- |
| **112** | Class for managing raw media content linkages. |
| **123-126** | `GetByLessonIdAsync`: Lists media items for a lesson in the correct sequence. |
| **131-134** | `GetByIdAsync`: Fetches content and its full parent chain (Lesson -> Chapter) for deep metadata access. |
| **159** | `SaveAsync`: Commits all changes made across any of the repositories to the physical DB. |

## Program Flow
When the Course Player loads, it uses these repositories in sequence (or via nested Includes) to build the navigation tree.
1. `GetByCourseIdAsync` (Chapters) -> `GetByChapterIdAsync` (Lessons) -> `GetByLessonIdAsync` (Content).

## Interview Questions
1. **Why group these repositories into one file?**
   *Answer: Because they are highly "Cohesive". You almost never interact with a Lesson without its parent Chapter, so keeping their data access logic together simplifies project navigation for the developer.*
2. **Explain the `Include` chain in `GetByIdAsync`.**
   *Answer: It uses `.Include(l => l.Chapter).Include(l => l.Contents)` to ensure that when you fetch a specific Lesson, you also get all the files attached to it and the metadata of the chapter it belongs to. This prevents multiple database trips.*
3. **What is the risk of having an empty `Task.CompletedTask` in `UpdateAsync`?**
   *Answer: It's an async "Shim". In EF Core, `_context.Update()` is a synchronous operation (it just marks the object as modified). The actual database trip only happens during `SaveChangesAsync`. The `Task.CompletedTask` is just there to satisfy the `async` interface requirement.*
4. **Why do we use an Interface (`IChapterRepository`, etc.) instead of just the class?**
   *Answer: Dependency Injection and Testing. By using an interface, we can easily swap out the SQL implementation for a "Mock" implementation during unit testing without changing the controller's code.*
5. **What is the benefit of `Async` methods in a repository?**
   *Answer: Scalability. Async methods allow the server to handle other requests while waiting for the database to respond, preventing "Thread Starvation" during heavy traffic.*
