# LearnSphere - Master Documentation

## Project Overview
LearnSphere is a full-stack, enterprise-grade Learning Management System (LMS) designed for a seamless student experience and powerful administrative control. It features a modern React frontend and a robust .NET Core backend.

## 🏗 System Architecture

### Backend (.NET Core 8)
The backend follows a strict **Clean Architecture** pattern with a clear separation of concerns:
- **Models**: Defines the core identity of the system (Users, Courses, Chapters, Lessons).
- **Repositories**: Manages the "Data Persistence" (SQL Server via Entity Framework).
- **Services**: Contains the "Business Logic" (File uploads, Eligibility math).
- **Controllers**: The "Gatekeepers" that expose APIs to the frontend.
- **DTOs**: Ensures that only safe, filtered data is sent over the network.

### Frontend (React & Tailwind)
The frontend is a fast, responsive Single Page Application (SPA):
- **Components**: 36+ reusable UI elements built for speed and aesthetics.
- **Pages**: 15 distinct views ranging from the complex Learning Player to the Admin Dashboard.
- **State Management**: Uses the custom `EnrollmentStore` (Subscription pattern) for high-performance, real-time data syncing.
- **API Services**: Centralized Axios configuration with automatic JWT authentication.

## 🌊 Detailed Program Flows (Life of a Request)

To understand how LearnSphere works, here is exactly what happens during key actions:

### 1. The "Life of a User Login"
1.  **Frontend**: User enters credentials in `LoginPage.jsx`.
2.  **API Layer**: `authApi.js` sends a POST request to `/api/auth/login`.
3.  **Backend Controller**: `AuthController.cs` receives the request and maps it to a `LoginDto`.
4.  **Service Layer**: `AuthService.cs` verifies the password hash using the `UserRepository`.
5.  **Token Generation**: If valid, the backend generates a **JWT (token)** and sends it back.
6.  **Persistence**: The frontend stores this token in `localStorage` and `http.js` automatically attaches it to all future requests.

### 2. The "Life of a Course Upload"
1.  **Frontend**: Admin uses the `CourseForm.jsx` to upload a video.
2.  **API Layer**: `courseApi.js` sends the video as `FormData`.
3.  **Backend Controller**: `CourseController.cs` receives the file.
4.  **File System**: The `FileUploadService.cs` validates the size, generates a unique GUID filename, and saves the bytes into `wwwroot/uploads`.
5.  **Database**: The service returns a URL (e.g., `http://localhost:5267/uploads/...mp4`), and the `CourseRepository` saves this URL into the `CourseContents` table.
6.  **Visual**: The video is now instantly streamable by any student.

### 3. The "Life of a Student Enrollment"
1.  **Frontend**: Student clicks "Enroll Now" on `CourseDetailPage.jsx`.
2.  **API Layer**: Calls `enrollCourseAPI` in `enrollmentApi.js`.
3.  **Backend**: `EnrollmentController` creates a new record in the `Enrollments` table via `AppDbContext`.
4.  **Reactive Update**: On success, the frontend calls `EnrollmentStore.enrollCourse()`. 
5.  **Sync**: Because the `Navbar` and `Dashboard` are "Subscribed" to the `EnrollmentStore`, they instantly re-render to show "My Courses" without a page refresh.

### 4. The "Life of a Final Assessment"
1.  **Frontend**: Student completes the last lesson in `CoursePlayerPage.jsx`.
2.  **Eligibility**: `AssessmentPlayer.jsx` calls `getEligibility`. The backend calculates if all `LessonProgress` records exist for this student.
3.  **Start**: Student clicks "Start". The backend creates an `AssessmentAttempt` with a `StartedAt` timestamp.
4.  **Submission**: Student submits answers.
5.  **Grading**: `AssessmentController` compares `StudentAnswers` against `CorrectAnswers` in the DB, calculates the percentage, and updates the `IsPassed` flag.
6.  **Result**: The UI instantly shows a Trophy or a "Try Again" message based on the score.

## 📁 Project Structure
```text
LearnSphere/
├── Backend/ (LearnSphereBackend)
│   ├── Controllers/   # API Endpoints
│   ├── Models/        # Database Schemas
│   ├── Services/      # Business Logic
│   └── Repositories/  # Data Access
├── Frontend/ (learn-sphere-ui)
│   ├── src/
│   │   ├── api/       # Network Services
│   │   ├── components/# Reusable UI
│   │   ├── pages/     # Feature Views
│   │   └── App.jsx    # Routing Hub
└── wwwroot/           # Course Material Storage
```

## 🛠 Tech Stack
- **Frontend**: React, Tailwind CSS, Axios, Lucide Icons.
- **Backend**: C#, ASP.NET Core, Entity Framework Core, SQL Server.
- **Storage**: Local filesystem for media (handled by `FileUploadService`).
- **Security**: JWT (JSON Web Tokens) with role-based access control.

## 🤝 Conclusion
LearnSphere is built for scalability and clarity. Every line of code is documented with its own README to help new developers understand the flow, connections, and logic behind this professional learning platform.
