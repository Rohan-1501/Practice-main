# Documentation: LiveSessionManager.jsx

## Overview
`LiveSessionManager.jsx` is the platform's scheduler for timed learning events and video uploads.

## Detailed Explanation
This component manages "Live Sessions", which can be either real-time events or pre-recorded videos that go live at a specific time. It features a modern "FileDropZone" that supports drag-and-drop for both Video files and Thumbnails.

## Program Flow
1. **Time Validation**: Before saving, it checks if the session ends before it starts and ensures that new sessions aren't scheduled in the past.
2. **FormData Submission**: Unlike other forms that send JSON, this one uses the `FormData` browser API because it needs to transmit "Binary" files (MP4s and JPGs) to the backend.
3. **Time Formatting**: It includes complex logic to "Normalize" ISO timestamps, ensuring that dates from the backend are correctly adjusted to the user's local timezone.

## Why it is used
To provide a "Scheduled" learning experience. It allows instructors to build anticipation for new content by announcing they will "Go Live" at a certain time.

## Interview Questions
1. **Why do you use `new FormData()` instead of a regular JSON object?**
   *Answer: File Support. JSON cannot natively carry binary files like videos or images. `FormData` is the standard way to send "Multipart" requests to a server, which is required for file uploads.*
2. **How does the `FileDropZone` handle user interaction?**
   *Answer: It uses three events: `onDragOver` (to show the highlight), `onDragLeave` (to hide it), and `onDrop` (to capture the file from the user's cursor). It also has a hidden `<input type="file">` so users can click to browse if they prefer.*
3. **Explain the `toLocalISOString` utility.**
   *Answer: Browser Compatibility. The HTML `<input type="datetime-local">` requires a very specific string format (YYYY-MM-DDTHH:mm). This function takes a standard UTC date and manually adjusts it by the user's timezone offset to ensure the input field displays the correct local time.*
  
