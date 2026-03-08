# Documentation: Course.cs

## Overview
`Course.cs` is a core domain model representing a learning course in the LearnSphere system. it defines the structure of course data stored in the database.

## Detailed Explanation
The file uses Entity Framework Core (EF Core) data annotations to define schema constraints. It tracks essential course information like title, pricing, level, and popularity (students enrolled).

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1** | `using ...DataAnnotations;` | Imports attributes for primary keys and validation. |
| **5** | `public class Course` | Defines the master entity for a course. |
| **7-8** | `[Key] public int Id ...` | Primary identity field for the course table. |
| **10-11** | `[Required] public string Title ...` | Enforces that every course must have a name. |
| **13** | `public string? Slug { ... }` | URL-friendly unique identifier (Safe for browsers). |
| **15** | `public string? Summary { ... }` | A pitch or snippet for the course card. |
| **17** | `public string? Description { ... }` | Comprehensive landing page content (HTML supported). |
| **19** | `public string? Thumbnail { ... }` | Relative server path to the course cover image. |
| **22** | `public string? Categories { ... }` | Searchable tags stored as a flat string. |
| **24** | `public string? Duration { ... }` | Human-readable time estimate (e.g., "15h 30m"). |
| **26** | `public string Level { ... }` | Categorization (Beginner, Intermediate, Advanced). |
| **28** | `public decimal Price { ... }` | Cost field with high financial precision. |
| **30** | `public int Students { ... }` | Cached enrollment count for fast data retrieval. |
| **32** | `public string Type { ... }` | Differentiates between self-paced and live sessions. |
| **34** | `public string Status { ... }` | Workflow state: Draft (Private) vs Published (Public). |
| **37** | `public DateTime CreatedAt ...` | Timestamp of original creation (Normalized to UTC). |
| **39** | `public DateTime UpdatedAt ...` | Timestamp of most recent edit (Normalized to UTC). |
| **42** | `public ICollection<Enrollment> ...` | Navigation to student-course junction records. |
| **45** | `public ICollection<Chapter> ...` | Navigation to the hierarchical curriculum content. |
| **46** | `public ICollection<LiveSession> ...` | Navigation to scheduled real-time interaction points. |

## Program Flow
1. **Data Definition**: When the application starts, EF Core uses this model to ensure the `Courses` table in the database matches this structure.
2. **Data Retrieval**: When a user browses courses, the `CoursesController` fetches data into this model format.
3. **Data Relationship**: It acts as a parent for `Chapters` and `LiveSessions`.

## Connections
- **Connected to `Enrollment`**: Via the mandatory `Enrollments` collection (One-to-Many).
- **Connected to `Chapter`**: A course contains multiple chapters.
- **Connected to `LiveSession`**: A course can have associated live sessions if it's not strictly self-paced.
- **Used by `CoursesController`**: To map database data to API responses.

## Properties Explanation
- `Id`: Primary key for the course record.
- `Title`: The name of the course. Required.
- `Slug`: URL-friendly version of the title.
- `Level`: Difficulty setting (beginner, intermediate, advanced). Default is beginner.
- `Price`: Cost of the course. Decimal type for financial accuracy.
- `Type`: Distinguishes between "Live" and "Self-Paced" courses.

## Why it is used
It provides a strongly-typed representation of a "Course" entity, ensuring data consistency across the backend logic and the database via an Object-Relational Mapper (ORM).

## Interview Questions
1. **What is the purpose of the `[Key]` attribute in this model?**
   *Answer: It tells EF Core that the `Id` property is the primary key for the database table.*
2. **Why use `decimal` for the `Price` property instead of `double` or `float`?**
   *Answer: Decimal is more precise for financial calculations and avoids rounding errors common with floating-point types.*
3. **How does the `ICollection` property help in managing relationships?**
   *Answer: It allows for "Lazy Loading" or "Eager Loading" of related data (like Chapters) when a Course is fetched.*
