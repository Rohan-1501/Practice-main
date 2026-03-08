# Documentation: Lesson.cs

## Overview
`Lesson.cs` represents an individual unit of learning within a chapter. it is the primary container for educational content like videos, documents, or text.

## Detailed Explanation
A lesson is a child of a `Chapter`. It contains a collection of `CourseContent` items, allowing a single lesson to have multiple resources (e.g., a video lecture plus a PDF handout). It also tracks its own sequence via the `Order` property.

## Program Flow
1. **Curriculum Design**: Admins create lessons and assign them to specific chapters.
2. **Learning Path**: when a student opens a course, the system loads lessons in the sequence defined by `Chapter -> Order` and `Lesson -> Order`.
3. **Content Delivery**: When a lesson is clicked, all associated `CourseContent` items are loaded into the player.

## Connections
- **Connected to `Chapter`**: Belongs to one parent Chapter.
- **Connected to `CourseContent`**: Can have multiple content items (video, doc, etc.).
- **Connected to `LessonProgress`**: Used to track if a specific student has completed this lesson.

## Properties Explanation
- `ChapterId`: Foreign key linking to the parent Chapter.
- `Title`: The name of the lesson.
- `Duration`: Represents how long the lesson takes (e.g., "10m 30s").
- `Order`: Determines the lesson's position within its chapter.

## Why it is used
It acts as the "granule" of the learning experience. It provides the necessary structure to group actual files and text into a coherent teaching step.

## Interview Questions
1. **How is a Lesson different from CourseContent?**
   *Answer: A Lesson is a structural unit (like a "page" in a book), while CourseContent is the actual material (like an "image" or "paragraph" on that page). A Lesson can contain many CourseContents.*
2. **What is the significance of the `Duration` field being a string?**
   *Answer: It allows for flexible formatting (like "45 mins" or "00:45:00") without strict integer constraints, though it makes calculating total course time slightly harder.*
3. **How does Entity Framework handle the deletion of a Chapter containing Lessons?**
   *Answer: By default, if Cascade Delete is enabled, deleting a Chapter will automatically delete all its associated Lessons to prevent "orphan" records.*
