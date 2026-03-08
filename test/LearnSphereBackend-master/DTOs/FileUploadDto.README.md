# Documentation: FileUploadDto.cs

## Overview
`FileUploadDto.cs` handles the results of file uploads and metadata for content creation.

## Detailed Explanation
- `FileUploadResponseDto`: Sent by the `FileUploadService` after a file is saved to the server. It includes the `FileUrl` and `FileSize`.
- `ContentUploadRequestDto`: Used to provide a `Title` and `Description` for the newly uploaded file.

## Program Flow
1. **Upload**: User sends a file.
2. **Response**: Backend sends `FileUploadResponseDto` confirming success.
3. **Save**: Frontend then sends `ContentUploadRequestDto` to attach that file to a specific lesson with a title and order.

## Why it is used
It provides a structured way to handle the "Two-Step" upload process (Upload File -> Name the Content).

## Interview Questions
1. **What is the purpose of the `Success` boolean in the response?**
   *Answer: It allows the frontend to quickly decide whether to show a green "Success" message or a red "Error" alert with the `Message` provided.*
2. **Why store `FileSize` in the DTO?**
   *Answer: So the frontend can display it to the user (e.g., "PDF - 2.5 MB"), which is helpful for students with limited data.*
