# Documentation: EnrolledCourses.jsx

## Overview
`EnrolledCourses.jsx` is the "My Learning" page where students manage their active and finished curriculum.

## Detailed Explanation
This page acts as a filter on the student's total academy history. It splits their courses into two clear categories:
1. **Enrolled**: Courses they are currently working on.
2. **Completed**: Courses where they have successfully passed the final assessment.

It provides a "Go to Course" button for easy access and an "Unenroll" option for students who wish to remove a course from their dashboard.

## Program Flow
1. **Reactive State**: Subscribes to the `EnrollmentStore` so if a user unenrolls, the card vanishes from the list instantly.
2. **List Filtering**: Uses standard JavaScript `.filter` to split the enrollment array into two sub-lists based on the `status` property.
3. **Reusable Card**: Uses a small internal `CourseCard` component to ensure both sections look consistent and share the same logic.

## Why it is used
Organization. As a student's library grows, they need a dedicated space to find the courses they actually care about, separate from the "Explore" catalog.

## Interview Questions
1. **Why is the "Unenroll" button hidden for completed courses?**
   *Answer: Business Logic. Once a student has achieved the "Completed" status, that record often represents a permanent achievement (like a certificate). We prevent accidental unenrollment to preserve that record of success.*
2. **Explain how `isCompleted` changes the navigation logic.**
   *Answer: Content context. If the course is completed, we append `/learn` to the URL. This takes the user directly back to the player in "Review Mode" rather than just the detail page, allowing them to instantly access their favorite lessons.*
3. **How does the page handle the "Empty State"?**
   *Answer: Conditional Rendering. If the `enrolledCourses` array is empty, it displays a friendly message suggesting the student explore the catalog. This is better than showing a blank screen, which might look like a bug.*
  
