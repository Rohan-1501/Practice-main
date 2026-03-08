# Documentation: CourseDto.cs

## Overview
`CourseDto.cs` contains different versions of Course data used for Creating, Updating, and Viewing courses.

## Detailed Explanation
It separates the "Request" (what the user sends to the server) from the "Response" (what the server sends to the user). For example, `CourseCreateRequestDto` doesn't include an `Id` because the database generates that later.

## Line-by-Line Explanation

## Line-by-Line Explanation: CourseDto.cs

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1** | `namespace MyProject.Api.DTOs;` | Organizes the model into the DTO layer. |
| **3** | `public class CourseDto` | Defines the base data structure for a course. |
| **5** | `public int Id { get; set; }` | Unique numeric identifier from the DB. |
| **6** | `public string Title { ... } = null!;` | The course name (Required, non-nullable). |
| **7** | `public string? Slug { ... }` | URL-friendly title (e.g., `intro-to-csharp`). |
| **8** | `public string? Summary { ... }` | A short, 1-sentence description. |
| **9** | `public string? Description { ... }` | Detailed, multi-paragraph content. |
| **10** | `public string? Thumbnail { ... }` | Link to the course cover image. |
| **11** | `public string? Categories { ... }` | Comma-separated list of academic tags. |
| **12** | `public string? Duration { ... }` | Total estimated time to complete. |
| **13** | `public string Level { ... } = "beginner";` | Difficulty rating (Beginner, Intermediate, Advanced). |
| **14** | `public decimal Price { ... } = 0;` | Enrollment cost (stored with high precision). |
| **15** | `public int Students { ... } = 0;` | Total number of students currently enrolled. |
| **16** | `public string Status { ... } = "published";` | Lifecycle state (Draft, Published, Archived). |
| **17** | `public DateTime CreatedAt { ... }` | Timestamp of when the record was first made. |
| **18** | `public DateTime UpdatedAt { ... }` | Rolling timestamp of the last modification. |

## Line-by-Line Explanation: CourseCreateRequestDto.cs

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **21** | `public class CourseCreateRequestDto` | Specialized model for brand-new course creation. |
| **23-32** | Class Properties | **Logic**: Omits `Id` and `Auditing` fields, as the client cannot specify these. |

## Line-by-Line Explanation: CourseResponseDto.cs

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **35** | `public class CourseResponseDto` | The official API response structure. |
| **37-50** | Class Properties | **Logic**: includes the auto-generated `Id` and `Auditing` fields for the UI. |

## Program Flow
1. **Create**: Frontend sends a `CourseCreateRequestDto`.
2. **Process**: Backend converts this DTO into a `Course` entity.
3. **Save**: Entity is saved to DB.
4. **Return**: The backend creates a `CourseResponseDto` (which now has an `Id` and `CreatedAt`) and sends it back.

## Why it is used
It protects the system from "Over-Posting" attacks. By using a DTO, we define exactly which fields a user is ALLOWED to change, preventing them from manually setting their own `Id` or `CreatedAt` date.

## Interview Questions
1. **Why do we have both `CourseDto` and `CourseResponseDto`?**
   *Answer: To maintain absolute control over the API contract. `CourseResponseDto` is what the public "sees", while `CourseDto` might be used for internal transfers.*
2. **What is an "Over-Posting" attack?**
   *Answer: It's when a user tries to update a field they aren't supposed to (like a "Price" or "IsAdmin" flag) by including it in the JSON request. DTOs prevent this by only acknowledging expected fields.*
3. **Why are some fields marked as `null!`?**
   *Answer: This is C# "Nullable Reference Type" syntax. It tells the compiler that even though the property is a string, it is required and should not be null in a valid DTO.*
