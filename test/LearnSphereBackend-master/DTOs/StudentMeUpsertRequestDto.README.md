# Documentation: StudentMeUpsertRequestDto.cs

## Overview
`StudentMeUpsertRequestDto.cs` is used by students to update their own profile information.

## Detailed Explanation
The term "Upsert" (Update + Insert) suggests that this DTO can be used both to fill in a new profile or modify an existing one. All properties are nullable to allow for partial updates (updating just the phone number, for instance).

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Namespace declaration. |
| **3** | `StudentMeUpsertRequestDto`: Incoming model for profile management. |
| **6-11** | **Personal Fields**: Optional set of individual user demographics. |
| **14-16** | **Academic Fields**: Optional update for school-related identity. |
| **19-22** | **Guardian Fields**: Optional update for parent/guardian contact info. |

## Why it is used
It gives the user control over their personal and academic data while keeping the interface flexible.

## Interview Questions
1. **What is a "Partial Update"?**
   *Answer: It is when a user only sends the fields they want to change. If a DTO has 20 fields but the user only sends 1, the backend should only update that 1 field.*
2. **Why are guardian details included in a "Student" profile?**
   *Answer: For safety and administrative reasons, especially in educational environments where emergency contact information is a requirement.*
