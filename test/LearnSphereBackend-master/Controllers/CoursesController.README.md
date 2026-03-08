# Documentation: CoursesController.cs

## Overview
`CoursesController.cs` is the most complex controller in the system. it manages the entire course lifecycle, including chapters, lessons, and physical file uploads.

## Detailed Explanation
This controller handles everything from listing basic course cards on the home page to managing the deep hierarchy of learning materials. It integrates with the `FileUploadService` to save videos and documents to the server's `wwwroot` directory.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **8** | `namespace MyProject.Api.Controllers;` | Organizes the API entry points. |
| **10** | `[ApiController]` | **Attribute**: Enables automatic model validation and HTTP 400 responses. |
| **11** | `[Route("api/courses")]` | **Attribute**: Sets the base URL for every endpoint in this class. |
| **12** | `public class CoursesController ...` | Main controller class inheriting from `ControllerBase`. |
| **14-18** | `private readonly ...` | **DI**: Private fields for injected Repositories and Services. |
| **20-35** | `public CoursesController(...)` | **Constructor**: Receives dependencies from the container. |
| **42-43** | `[HttpGet] [AllowAnonymous]` | **Access**: Public endpoint to list courses without a token. |
| **44** | `public async Task<IActionResult> GetAll()` | Fetches all courses asynchronously. |
| **48** | `await _courseRepo.GetAllAsync()` | **Data**: Hits the database for the course list. |
| **49-65** | `Select(c => new CourseResponseDto ...)` | **Mapping**: Converts internal models to lean API DTOs. |
| **72** | `return Ok(dtos);` | Returns the JSON list with a 200 status code. |
| **77-78** | `[HttpGet("{id}")] [AllowAnonymous]` | Public GET for a specific course by its unique ID. |
| **81** | `_courseRepo.GetByIdAsync(id)` | Queries the specific course from the registry. |
| **82** | `if (course == null) return NotFound(...)` | **Guard**: Returns 404 if the course doesn't exist. |
| **108-110** | `[HttpPost] [Authorize] Create(...)` | **Action**: Protected endpoint to insert a new course. |
| **113-117** | `if (!User.IsInRole("admin"))` | **Security**: Explicitly blocks non-admin users from creating content. |
| **129-143** | `new Course { ... }` | **Conversion**: Maps the DTO into a persistent database entity. |
| **145** | `await _courseRepo.AddAsync(course)` | **Persistence**: Commits the new course record to SQL. |
| **150-163** | `foreach (var student in students)` | **Integration**: Loop to trigger notifications for all enrolled students. |
| **195-197** | `[HttpPut("{id}")] [Authorize] Update(...)` | **Action**: Allows admins to modify existing course metadata. |
| **244-246** | `[HttpDelete("{id}")] [Authorize] Delete(...)` | **Action**: Removes a course and all related hierarchical data. |
| **271-275** | `GetCourseStructure(int id)` | **API**: Fetches the deep tree (Chapters -> Lessons -> Content). |
| **283-328** | `foreach (var chapter in chapters)` | **Logic**: Recursive loops to build the full curriculum JSON object. |
| **734-745** | `UploadContent(...)` | **API**: Handles `multipart/form-data` for binary file uploads (Videos/PDFs). |
| **774-775** | `UploadFileAsync(...)` | **Service**: Calls the file system to save the physical bits. |
| **781-793** | `new CourseContent { ... }` | **Metadata**: Records the new file URL in the database. |

## Program Flow
1. **Public View**: Provides an anonymous endpoint to list published courses.
2. **Structure Management**: Allows admins to add/edit/delete Chapters and Lessons.
3. **Content Upload**: 
   - Receives a multipart form request with a file.
   - Saves the file to disk via a service.
   - Records the metadata (URL, Size, Type) in the `CourseContent` table.
4. **Course Structure**: Sends the "Big Object" (Course -> Chapter -> Lesson) to the course player.

## Connections
- **Depends on `ICourseRepository`, `IChapterRepository`, `ILessonRepository`, and `IContentRepository`**.
- **Depends on `IFileUploadService`** for physical storage.
- **Used by both the Admin course editor and the Student course player**.

## Why it is used
It is the backbone of the entire learning platform. Without this controller, there would be no way to organize or deliver the educational content.

## HTTP GET Methods: Detailed Explanation

### 1. `GetAll()`
- **Endpoint**: `GET /api/courses`
- **Access**: Public (`AllowAnonymous`)
- **Working**: This method acts as the "Catalog API". It instructs the `CourseRepository` to fetch every course in the database. It then maps the database models into a list of `CourseResponseDto` objects, stripping away any sensitive internal data and providing a clean list for the landing page or search results.

### 2. `GetById(int id)`
- **Endpoint**: `GET /api/courses/{id}`
- **Access**: Public (`AllowAnonymous`)
- **Working**: Used for the "Course Preview" page. It takes a unique course ID from the URL and asks the repository for that specific course. If found, it returns a single `CourseResponseDto`. This is the first step a student takes before enrolling, as it gives them the high-level details of what the course covers.

### 3. `GetCourseStructure(int id)`
- **Endpoint**: `GET /api/courses/{id}/structure`
- **Access**: Public (`AllowAnonymous`)
- **Working**: This is a "Heavy Loader" method. Instead of just course metadata, it recursively fetches the entire tree of learning materials: Chapters -> Lessons -> Content (Videos, PDFs, etc.). It is used by the Student Course Player to build the navigation sidebar.

### 4. `GetChapterById(int courseId, int id)`
- **Endpoint**: `GET /api/courses/{courseId}/chapters/{id}`
- **Access**: Public (`AllowAnonymous`)
- **Working**: Fetches a deep-dive view of a specific chapter. It retrieves the chapter details and then queries for every lesson associated with it, including the media contents for those lessons.

### 5. `GetLessonById(int courseId, int chapterId, int id)`
- **Endpoint**: `GET /api/courses/{courseId}/chapters/{chapterId}/lessons/{id}`
- **Access**: Public (`AllowAnonymous`)
- **Working**: Targeted fetch for a specific lesson. It retrieves the lesson data and immediately includes its associated `CourseContent` (the actual video URL or document path).

## HTTP POST, PUT & DELETE Methods: Detailed Explanation

### 1. `Create()`
- **Endpoint**: `POST /api/courses`
- **Access**: Admin Only
- **Working**: Creates a new course object. It validates that the user is an admin, generates a `CreatedAt` timestamp, and saves the new record. It also triggers a **Global Notification** to all students to announce the new course.

### 2. `Update(int id)`
- **Endpoint**: `PUT /api/courses/{id}`
- **Access**: Admin Only
- **Working**: Modifies existing course metadata (Title, Summary, etc.). It uses the `ICourseRepo` to perform an "Update" operation on the database record identified by the ID.

### 3. `Delete(int id)`
- **Endpoint**: `DELETE /api/courses/{id}`
- **Access**: Admin Only
- **Working**: Removes the course and **ALL linked chapters/lessons** from the database (Cascading Delete). It also calls `NotificationsController` to clean up any alerts related to this course.

### 4. `CreateChapter()` & `CreateLesson()`
- **Endpoints**: `POST /api/courses/{id}/chapters`, `POST .../lessons`
- **Working**: Inserts a new node into the course hierarchy. These methods ensure that a lesson is always attached to a valid chapter, and a chapter to a valid course.

### 5. `UpdateChapter()` & `UpdateLesson()`
- **Endpoints**: `PUT /api/courses/.../chapters/{id}`, `PUT .../lessons/{id}`
- **Working**: Allows admins to rename titles, swap the `Order` index (for drag-and-drop reordering), and update descriptions.

### 6. `DeleteChapter()` & `DeleteLesson()`
- **Endpoints**: `DELETE /api/courses/.../chapters/{id}`, `DELETE .../lessons/{id}`
- **Working**: Removes the specific unit. If a chapter is deleted, all its lessons are also deleted automatically.

### 7. `UploadContent()`
- **Endpoint**: `POST /api/courses/.../lessons/{lessonId}/content`
- **Access**: Admin Only
- **Working**: This is a **Multipart Form** handler. It receives a physical file (e.g., MP4), calls `FileUploadService` to save it to disk, and then creates a database entry in the `CourseContents` table with the newly generated URL.

## Interview Questions
1. **How is the "Content Upload" flow different from a typical "Data" flow?**
   *Answer: Content upload involves both a Database record AND a Physical File on disk. The controller must ensure that if the database save fails, the file is cleaned up, or vice versa, to prevent orphaned files.*
2. **Explain the purpose of the `Slug` property.**
   *Answer: It creates a SEO-friendly and human-readable URL (e.g., `/courses/intro-to-react`) instead of using a generic ID (e.g., `/courses/105`).*
3. **Why do we use `AllowAnonymous` on some methods and `Authorize` on others in the same controller?**
   *Answer: To support different user roles. Landing pages (listing courses) should be public to attract new students, while management features (adding lessons) must be restricted to admins.*
4. **What is `IActionResult` and why do we use it as a return type?**
   *Answer: `IActionResult` is an interface that allows a method to return various types of HTTP responses (like `Ok()`, `NotFound()`, `BadRequest()`). It provides flexibility to return data along with the correct HTTP status code.*
5. **What are "Attributes" (the things in square brackets like `[HttpGet]`)?**
   *Answer: Attributes are metadata tags applied to classes or methods. They tell the ASP.NET Core runtime how to treat the code—for example, `[Route]` defines the URL path, and `[ApiController]` enables automatic model validation.*
6. **In simple terms, what is an "API" in the context of this controller?**
   *Answer: An API (Application Programming Interface) is a "Contract" or a "Waiter" that takes a request from the frontend (the customer), fetches data from the database (the kitchen), and delivers it back in a format both can understand (JSON).*
