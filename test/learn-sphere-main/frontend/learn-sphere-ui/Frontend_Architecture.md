# LearnSphere Frontend Architecture: The Component-Driven UI

## Overview
The LearnSphere user interface is a modern **Single Page Application (SPA)** built with **React** and **Vite**. It prioritizes a "premium" feel with dark mode, glassmorphism, and smooth micro-animations.

## 🏁 Starting Point: main.jsx & App.jsx
1. **main.jsx**: The bootstrapper that attaches the React app to the HTML `index.html`.
2. **App.jsx**: The "Grand Central Station" of the frontend. It defines the **Routes** (which page shows up for which URL) and the **Authentication Guard**.

## 🏗️ Folder Structure (The Layers)
- **`/api`**: The "Post Office". Contains `http.js` (Axios setup) and service-specific files (`courseApi.js`). It injects the JWT token into every outgoing request automatically.
- **`/pages`**: Full-screen views (e.g., `LandingPage.jsx`, `CoursePlayerPage.jsx`). These act as the "Parent" components that fetch data.
- **`/components`**: Reusable UI blocks.
  - **`/course`**: Specialized players for Quizzes and Assessments.
  - **`/dashboard`**: Shared layout items like the Sidebar.

## 🔄 Data Handling & request Flow
1. **Trigger**: A user clicks a button (e.g., "Start Quiz").
2. **Fetch**: The Page calls an API service (e.g., `quizApi.getQuiz()`).
3. **Intercept**: `http.js` adds the `Authorization: Bearer <TOKEN>` header from `localStorage`.
4. **State Update**: Once the backend returns data, the Page updates its **React State** (`useState`).
5. **Re-render**: React detects the state change and automatically updates only the parts of the screen that changed.

## 💎 Design Philosophy: "Wow the User"
- **Glassmorphism**: Headers and sidebars use semi-transparent backgrounds with back-drop blur for a premium look.
- **Micro-Animations**: The `CoursesAdmin` tab transitions and the `LandingPage` marquee provide a sense of life to the app.
- **Conditional Loading**: Components like the `AssessmentPlayer` are gated based on progress, creating a "Gamified" learning path.

## 🛡️ Key Technologies
- **Tailwind CSS**: For high-speed, modern styling.
- **Lucide-React & FontAwesome**: For consistent, accessible iconography.
- **Axios**: For robust API communication and global error handling.
- **React Router**: For seamless, non-refreshing page transitions.

---
*This document acts as the high-level map for the frontend. For line-by-line details, please refer to the individual `.README.md` files in each folder.*
