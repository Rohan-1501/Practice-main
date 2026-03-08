# Documentation: UpdateUserStatusRequestDto.cs

## Overview
`UpdateUserStatusRequestDto.cs` is an administrative tool used to toggle a user's account status.

## Detailed Explanation
It contains a single string `Status` (usually "active" or "inactive"). It is used in the Admin panel to disable or re-enable accounts.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Namespace declaration. |
| **3** | `UpdateUserStatusRequestDto`: defines the shape for status updates. |
| **5** | `Status`: The target state (active/inactive) for the account. |

## Interview Questions
1. **Why create a whole DTO for just one field?**
   *Answer: Consistency. Standardizing all API requests as JSON objects (even if they have one field) makes the API easier to maintain and use with automated tools.*
2. **What is the business impact of setting a status to "inactive"?**
   *Answer: Usually, the login logic will check this status and deny access to the system, even if the user provides the correct password.*
