# Documentation: CourseForm.jsx

## Overview
`CourseForm.jsx` is a high-powered editor for creating and modifying courses.

## Detailed Explanation
This is one of the most complex UI components in the project. It features:
- **Interactive Image Uploader**: Supports both "Click to Browse" and "Drag and Drop" for thumbnails.
- **Dynamic Slug Generation**: A "Generate" button that automatically creates a URL-friendly slug from the course title.
- **Rich Text Integration**: Incorporates the `VisualEditor` for the course description.
- **Validation**: Integrates with `titleValidator.js` to ensure course titles don't contain illegal characters.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **8-18** | **Props**: Destructures the form state, error objects, and category options. |
| **19-20** | **Refs**: Tracks the hidden file input and the visual drop zone for thumbnails. |
| **25-37** | **Change Handler**: Updates state; includes logic for parsing numbers and real-time title validation. |
| **39-53** | **File Logic**: Validates MIME type and uses `FileReader` to generate a Base64 preview string. |
| **55-85** | **Drag & Drop**: Lifecycle methods for managing the drag state and capturing dropped files. |
| **92-95** | **Layout**: Wraps the form in a card-like container with a dynamic title (New/Edit). |
| **97-119** | **Title Input**: Controlled input with real-time feedback and warning labels. |
| **121-154** | **Slug Input**: Includes a "Generate" button to normalize the title into a URL. |
| **167-179** | **Summary**: Basic text field for the one-line description. |
| **181-242** | **Image Upload**: Interactive area for thumbnails (Preview or Drop Zone). |
| **244-261** | **Detailed Desc**: Integrates the `VisualEditor` for rich text content. |
| **263-275** | **Categories**: Uses `SearchableSelect` for multi-tag management. |
| **277-289** | **Level**: Dropdown for beginner/intermediate/advanced tiers. |
| **306-318** | **Status**: Select menu for Published/Draft/Archived states. |
| **320-335** | **Actions**: Save/Cancel buttons; Save is disabled if validation fails. |

## Program Flow
1. **Input Handling**: Centralized `onChange` function that parses numbers and triggers real-time title validation.
2. **File Processing**: Uses `FileReader` to convert uploaded images into a "Base64" preview string instantly.
3. **Drag-and-Drop**: Manages `dragActive` state to change the UI border color when a user hovers a file over the drop zone.
4. **Submission**: The `onSave` callback is only enabled if the form passes internal `canSave` checks.

## Why it is used
It simplifies the complex task of entering course metadata. Instead of a boring list of inputs, it provides a modern, interactive experience with instant previews.

## Interview Questions
1. **Explain the "Drag and Drop" logic used here.**
   *Answer: It uses the HTML5 Drag and Drop API. We handle `onDragOver` (to prevent the browser from opening the file), `onDragEnter/Leave` (to toggle the UI highlight), and `onDrop` (to capture the file object).*
2. **What is the `FileReader` and why is it used?**
   *Answer: It is a browser API that allows web apps to read the contents of files stored on the user's computer. We use `readAsDataURL` to create a local temporary URL for the image so the user can see their thumbnail BEFORE it's uploaded to the server.*
3. **How is the `VisualEditor` linked to this form?**
   *Answer: It acts like a "Controlled" input. We pass it the `form.description` as the value and a callback that updates only that specific field in the form state when the user types in the rich text area.*
