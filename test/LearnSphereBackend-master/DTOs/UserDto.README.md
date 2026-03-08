# Documentation: UserDto.cs

## Overview
`UserDto.cs` is the most comprehensive user-related DTO, combining identity data with the student profile.

## Detailed Explanation
It is typically used by admins to view a list of users. It includes the `User` data plus an optional `StudentMeResponseDto` if the user has a student profile attached.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Namespace declaration. |
| **3** | `UserDto`: Composite view model for user management. |
| **5** | `Id`: Unique Guid of the user record. |
| **6-7** | **Identity**: human-readable name and account email. |
| **8-9** | **Access**: Role (Admin/Student) and current account status. |
| **10** | `CreatedAt`: Date the account was originally created. |
| **11** | **Nested DTO**: Link to the extended student profile (if it exists). |

## Connections
- **Nests `StudentMeResponseDto`** to provide a full view of the person in one object.

## Interview Questions
1. **What is the benefit of nesting DTOs like this?**
   *Answer: It allows for an "Object Graph" response. Instead of fetching a User and then making a second call for their Student details, both are returned together.*
2. **When would `StudentProfile` be null in this DTO?**
   *Answer: When the user is an Admin or an Instructor who hasn't been assigned a student role.*
