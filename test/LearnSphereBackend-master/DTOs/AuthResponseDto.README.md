# Documentation: AuthResponseDto.cs

## Overview
`AuthResponseDto.cs` defines the structure of the data returned to a user after a successful login or registration.

## Detailed Explanation
This is a critical security object. It contains the `Token` (JWT) that the frontend must save to authenticate future requests, along with basic user info like `Name` and `Role`.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1** | `namespace MyProject.Api.DTOs;` | Organizes the model into the DTO layer. |
| **3** | `public class AuthResponseDto` | Defines the shape of the successful login handshake. |
| **5** | `public string Token { ... } = "";` | The signed JWT string for subsequent API authorization. |
| **6** | `public string Name { ... } = "";` | Human-readable user name for the UI header. |
| **7** | `public string Email { ... } = "";` | User's email address (used for profile context). |
| **8** | `public string Role { ... } = "";` | Categorization (Admin/Student) for client-side routing. |

## Program Flow
1. **Login**: User submits credentials.
2. **Validation**: Backend verifies the password.
3. **Generation**: `JwtTokenService` creates a signed token.
4. **Response**: The `AuthController` wraps the token and user details into this DTO and sends it back with a HTTP 200 OK.

## Why it is used
It provides the frontend with everything it needs to establish a session: the "Key" (Token) and the "Identity" (Name, Role).

## Interview Questions
1. **Why is the `Password` not included in this DTO?**
   *Answer: Security. You should never send a password (even hashed) back to the client after authentication is complete.*
2. **How does the frontend use the `Role` property from this DTO?**
   *Answer: It uses it for "Client-Side Authorization", such as hiding the "Admin Dashboard" link if the role is "student".*
3. **What happens if the `Token` is missing from this response?**
   *Answer: The frontend won't be able to authenticate subsequent API calls, and the user will effectively remain "logged out".*
