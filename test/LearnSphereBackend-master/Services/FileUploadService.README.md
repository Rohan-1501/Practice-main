# Documentation: FileUploadService.cs

## Overview
`FileUploadService.cs` manages the physical storage of media files on the server.

## Detailed Explanation
This service handles the "Dirty Work" of file management. It:
- **Validates**: Checks file size (max 500MB) and extensions (e.g., only `.mp4` for videos).
- **Stores**: Saves files into organized subfolders within the `wwwroot/uploads` directory.
- **Deletes**: Removes files from disk when a course or lesson is deleted.

## Line-by-Line Explanation: IFileUploadService.cs

| Line(s) | Explanation |
| :--- | :--- |
| **3** | Defines the interface for the media management service. |
| **5-9** | `UploadFileAsync`: Contract for processing `IFormFile` stream and returning metadata (URL, size). |
| **11** | `DeleteFileAsync`: Contract for removing physical files via their public URL. |
| **13** | `GetContentTypeFromMimeType`: Helper to categorize files (video, audio, etc.). |

## Line-by-Line Explanation: FileUploadService.cs

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **5** | `public class FileUploadService...` | Decouples the logic by implementing the interface. |
| **7** | `private readonly IWebHostEnvironment...` | Gets the server's root path for defining where files land. |
| **8** | `private readonly IConfiguration...` | Reads the Base URL from settings for link generation. |
| **9** | `AllowedVideoExtensions` | **Guard**: Only allows specific video formats (mp4, mov, etc.). |
| **10** | `AllowedAudioExtensions` | **Guard**: Only allows specific audio formats (mp3, wav, etc.). |
| **11** | `AllowedDocumentExtensions` | **Guard**: Only allows PDF and MS Office files. |
| **12** | `AllowedImageExtensions` | **Guard**: Only allows common web image formats. |
| **13** | `MaxFileSize` | **Policy**: Limits uploads to 500 MB per file. |
| **15-19** | `public FileUploadService(...)` | Standard constructor for dependency injection setup. |
| **21** | `UploadFileAsync(...)` | Asynchronous method to process an uploaded file. |
| **30-31** | `if (file == null || ...)` | **Security**: Immediate fail if the upload stream is corrupted. |
| **33-34** | `if (file.Length > MaxFileSize)` | **Security**: Immediate fail if file is excessively large. |
| **36** | `Path.GetExtension(...)` | Retrieves the file type (e.g., `.pdf`) to verify extension. |
| **40-41** | `if (!IsValidFileType(...))` | **Security**: Cross-references extension with requested category. |
| **46-50** | `if (courseId.HasValue)` | Organizes files by Course ID in sub-directories. |
| **53-55** | `else { ... }` | Default storage path for miscellaneous or live files. |
| **57-58** | `if (!Directory.Exists(...))` | Ensures the physical folder exists on the server disk. |
| **61** | `var uniqueFileName = ...` | Generates a GUID + Ticks to prevent name collisions. |
| **62** | `Path.Combine(...)` | Merges the folder path and unique name for the final address. |
| **65-68** | `using (var stream = ...)` | Creates a new file on disk and copies the data into it. |
| **71** | `var baseUrl = ...` | Resolves the host address (e.g., http://localhost:5267). |
| **72** | `var fileUrl = ...` | Combines baseUrl + path + filename into a clickable link. |
| **74** | `return (true, ...)` | Success result containing all file metadata. |
| **82** | `DeleteFileAsync(...)` | Method to safely remove files from the server hardware. |
| **90-91** | `new Uri(fileUrl)` | Parses the internal path from the full public URL. |
| **92** | `Path.Combine(...)` | Maps that internal path back to a physical disk location. |
| **94-98** | `if (File.Exists(...)) { ... }` | Deletes the file if it survives the validation checks. |
| **108-121** | `GetContentTypeFromMimeType(...)` | Maps standard browser types to system categories. |
| **123-133** | `IsValidFileType(...)` | Internal validation logic for security whitelists. |

## Program Flow
1. **Receipt**: Receives an `IFormFile` from a controller.
2. **Path Creation**: Uses `IWebHostEnvironment` to find the physical path on the server hardware.
3. **Unique Naming**: Renames every file using a `Guid` and `Ticks` to prevent users from overwriting each other's files.
4. **Link Generation**: Returns a full URL that the frontend can use to play the video or show the image.

## Why it is used
To provide a central, safe way to handle files. It ensures that uploaded files are never "Lost" and that the server doesn't get clogged with malicious or oversized files.

## Interview Questions
1. **What is `IWebHostEnvironment` and why is it used?**
   *Answer: It is a built-in ASP.NET Core service that provides info about the web hosting environment. Specifically, it gives us the path to the `wwwroot` folder, which is the only folder publicly accessible for serving files.*
2. **Why do we rename files to GUIDs instead of keeping the user's filename?**
   *Answer: For security and uniqueness. A user might upload "Lesson1.mp4". If another user uploads a different "Lesson1.mp4", the first one would be overwritten. GUIDs ensure every file has a 100% unique name.*
3. **What are the security risks of allowing file uploads?**
   *Answer: Users could upload viruses or "Zip Bombs". This service mitigates this by restricting allowed extensions and enforcing a strict file size limit.*
4. **Why do we use an Interface (`IFileUploadService`) instead of just the class?**
   *Answer: Dependency Injection and Testing. By using an interface, we can easily swap out the SQL implementation for a "Mock" implementation during unit testing without changing the code.*
5. **What is the benefit of `Async` methods in a repository?**
   *Answer: Scalability. Async methods allow the server to handle other requests while waiting for the database or disk to respond, preventing "Thread Starvation" during heavy traffic.*
