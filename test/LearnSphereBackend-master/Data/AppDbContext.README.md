# Documentation: AppDbContext.cs

## Overview
`AppDbContext.cs` is the "Schema Definition" and "Data Access Hub" for the entire LearnSphere backend. It is the bridge between C# code and the SQL Server database.

## Detailed Explanation
Using **Entity Framework Core**, this file defines:
- **DbSets**: Every property like `DbSet<Course> Courses` tells the system to create a corresponding table in the database.
- **Fluent API**: Inside `OnModelCreating`, we write the "Rules" of the database. For example, we enforce that every `User` must have a unique `Email` and define exactly how `Courses`, `Chapters`, and `Lessons` are linked together.
- **Cascading Deletes**: We configure the system so that the related chapters and lessons are also automatically removed if a course is deleted.
- **Type Conversions**: It includes modern logic to handle `DateOnly` properties (which SQL Server natively struggles with) and ensures all `DateTime` values are stored in **UTC format** to prevent timezone bugs.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **7** | `public class AppDbContext : DbContext` | Defines the main database context for the application. |
| **9** | `public AppDbContext(...)` | Constructor that passes configuration (connection strings) to the base. |
| **11-17** | `public DbSet<User> Users...` | **Tables**: Defines the core SQL tables for Users, Courses, and Lessons. |
| **20-22** | `public DbSet<Quiz> Quizzes...` | **Tables**: Defines the formative testing infrastructure. |
| **25-27** | `public DbSet<Assessment>...` | **Tables**: Defines the final exam (gatekeeper) infrastructure. |
| **30** | `public DbSet<LessonProgress>` | **Table**: Tracks completion status for every student/lesson pair. |
| **33-34** | `public DbSet<LiveSession>...` | **Tables**: Supports real-time peer learning sessions. |
| **39-41** | `modelBuilder.Entity<User>().HasIndex(...)` | **Logic**: Enforces unique emails at the database layer for security. |
| **44-47** | `modelBuilder.Entity<User>().HasOne(...)` | **Relation**: 1-to-1 link between a generic User and Student profile. |
| **50-54** | `modelBuilder.Entity<Student>().HasMany(...)` | **Relation**: 1-to-Many Enrollment link with Cascade delete enabled. |
| **64-66** | `.HasIndex(e => new { ... })` | **Logic**: A composite index prevents duplicate course enrollments. |
| **69-80** | `Course -> Chapter -> Lesson` | **Relation**: Hierarchical structure with cascading deletes. |
| **83-87** | `Lesson -> CourseContent` | **Relation**: Links lessons to their physical media (Videos, PDFs). |
| **104-108** | `Student -> QuizAttempt` | **Relation**: "NoAction" delete behavior to prevent accidental data loss. |
| **117-122** | `Course <-> Assessment` | **Relation**: Direct 1-to-1 link for the final course exam. |
| **146-160** | `LessonProgress` | **Logic**: Unique index ensures only one progress record per lesson per student. |
| **163-170** | `ValueConverter<DateOnly...>` | **Shim**: Modern C# fix to allow DateOnly storage in SQL Server. |
| **172-174** | `.HasPrecision(18, 2)` | **Logic**: Enforces high-precision decimal storage for course prices. |
| **196-199** | `ValueConverter<DateTime...>` | **Sync**: Forces every date in the DB to be UTC to prevent skew. |
| **201-210** | `foreach (var entityType...)` | **Automator**: Applies the UTC rule to every single entity automatically. |

## Program Flow
1. **Startup**: The backend registers this class in `Program.cs`.
2. **Migrations**: When you run a migration command, EF Core looks at this file to generate the SQL scripts needed to update the database schema.
3. **Runtime**: Every time a controller needs data, it uses an instance of `AppDbContext` to query the database.

## Why it is used
To ensure data integrity and simplified queries. By defining relationships (like `1-N` between Chapter and Lesson) here, we allow the code to use `.Include(c => c.Lessons)` to fetch related data in a single, efficient query.

## Interview Questions
1. **What is the purpose of `OnModelCreating`?**
   *Answer: Fine-grained Configuration. While EF Core can often "Guess" the database structure using conventions, `OnModelCreating` allows us to explicitly define complex rules, such as unique indexes, precision for currency (Price), and exact foreign key delete behaviors (Cascade vs SetNull).*
2. **How does the global UTC converter work?**
   *Answer: Property Normalization. The code loops through every `DateTime` property in every entity and attaches a `ValueConverter`. This ensures that whenever a date is saved, it is converted to UTC, and whenever it's read, it's treated as UTC. This is a best practice for apps used across multiple timezones.*
3. **Explain the unique constraint on Enrollment.**
   *Answer: Business Rule Enforcement. We use a "Composite Index" on `StudentId` and `CourseId` marked as `IsUnique()`. This prevents a database-level bug where a student could accidentally be enrolled in the same course twice, which would break their progress tracking.*
  
