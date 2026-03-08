# Documentation: Api.jsx (Registration Mock)

## Overview
`Api.jsx` is a "Simulator" that acts as a mock database during frontend development.

## Detailed Explanation
It uses `localStorage` to create a permanent, browser-based user store. This allows developers to test the registration, login, and profile update flows even when the backend server is offline. It features "Artificial Delays" (e.g., `delay(100)`) to simulate the real-world wait time of a network request.

## Program Flow
1. **Seeding**: On first load, it populates `localStorage` with a few "Demo" accounts (Student, Instructor).
2. **Hydration**: It reads all users from the browser into an in-memory `Map` for fast searching.
3. **Operations**:
   - `checkDuplicateEmail`: Scans the Map for an existing key.
   - `registerUser`: Adds a new user to the Map and persists the whole Map back to `localStorage`.

## Why it is used
For "Disconnected Development" and rapid prototyping. It allows the frontend team to build and test the entire user management system without waiting for the backend API to be finished.

## Interview Questions
1. **Why use a `Map` instead of a standard Object to store users?**
   *Answer: Performance and API. Maps are designed for frequent additions and lookups by key (email). They also provide helpful methods like `.has()` and `.clear()` which make the code more readable than manually checking object properties.*
2. **What is the purpose of the `delay` function?**
   *Answer: Realism. Real networks aren't instant. By adding a 100ms delay, we force the UI to show loading spinners, allowing us to test that the "Registering..." button state actually works and looks good.*
3. **Is this mock API safe for production?**
   *Answer: Absolutely not. It stores sensitive data like passwords in `localStorage`. This is for **Development Only**. In production, these mock functions are replaced with real AXIOS calls that communicate with the secure C# backend.*
