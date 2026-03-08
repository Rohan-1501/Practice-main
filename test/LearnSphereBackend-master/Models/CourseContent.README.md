# Documentation: CourseContent.cs

## Overview
`CourseContent.cs` represents the actual file or resource attached to a lesson. This is where the physical "material" (video, PDF, image) is defined.

## Detailed Explanation
This model stores the path to the physical file (`FileUrl`) and metadata about the file itself. It supports multiple types of media, categorized by `ContentType`.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1-2** | Imports for validation and schema mapping attributes. |
| **6** | Defines the `CourseContent` class for media attachments. |
| **9** | `Id`: Primary key for the content record. |
| **12-15** | **Hierarchy**: Links this specific file to a parent `Lesson`. |
| **18** | `Title`: The display name of the resource (e.g., "Lecture Video"). |
| **20** | `Description`: Internal or student-facing text about the resource. |
| **23** | `ContentType`: Discriminator for the type of media (video, audio, PDF). |
| **26** | `FileUrl`: The storage link (relative or absolute) for the file. |
| **28** | `FileName`: The original human-readable filename for downloads. |
| **30** | `FileSize`: Size in bytes, useful for auditing and UI display. |
| **32** | `Order`: Integer used to sequence files within the lesson. |
| **34-36** | **Timestamps**: Audit trail for creation and last update. |

## Program Flow
1. **Upload**: An admin uploads a file. The `FileUploadService` saves it to `wwwroot` and returns a URL.
2. **Storage**: The `CoursesController` creates a `CourseContent` record with that URL.
3. **Streaming/Viewing**: The frontend uses the `FileUrl` to render the appropriate player (e.g., `<video>` for videos, `<iframe>` for PDFs).

## Connections
- **Connected to `Lesson`**: Each content item is part of a Lesson.
- **Integrated with `FileUploadService`**: The service provides the values for `FileUrl`, `FileName`, and `FileSize`.

## Properties Explanation
- `ContentType`: A string discriminator (video, audio, document, image).
- `FileUrl`: The absolute or relative web path to the file.
- `FileName`: The original name of the file (useful for download links).
- `FileSize`: Stored in bytes for administrative reporting.

## Why it is used
It abstracts the physical file storage away from the curriculum structure. It allows a lesson to be composed of multiple diverse media types in a specific order.

## Interview Questions
1. **Why store a `FileUrl` instead of the actual file (BLOB) in the database?**
   *Answer: Database performance. Storing large files in a DB makes backups slow and increases memory usage. Storing them on the filesystem (or cloud storage) and keeping just the URL in the DB is more scalable.*
2. **What is the purpose of the `ContentType` property?**
   *Answer: It acts as a "hint" for the frontend so it knows whether to render a video player, an audio player, or a document viewer.*
3. **How would you handle a file moving to a different server?**
   *Answer: Since we store a `FileUrl`, we would only need to run a simple SQL update to change the base domain of the URLs, rather than migrating binary data.*
