# Documentation: AuthController.cs

## Overview
`AuthController.cs` is the entry point for user access. It handles registration and login requests.

## Detailed Explanation
The controller is thin, meaning it delegates the actual logic (password hashing, JWT generation) to the `IAuthService`. It focuses on handling HTTP requests and returning the appropriate status codes (200 OK, 400 Bad Request, 401 Unauthorized).

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **5** | `namespace MyProject.Api.Controllers;` | Organizes the API entry points. |
| **7** | `[ApiController]` | **Attribute**: Enables automatic model validation for login/register inputs. |
| **8** | `[Route("api/auth")]` | **Attribute**: Defines the base URL for the authentication system. |
| **9** | `public class AuthController ...` | Controller class inheriting from `ControllerBase`. |
| **11** | `private readonly IAuthService _auth;` | **DI**: Reference to the core authentication business logic. |
| **13** | `public AuthController(...)` | **Constructor**: Injects the service (keeps the controller "Thin"). |
| **15** | `[HttpPost("register")]` | **Action**: Public POST endpoint to create a new user account. |
| **16** | `Register(RegisterRequestDto req, ...)` | Handlers incoming registration packets. |
| **20** | `await _auth.RegisterAsync(...)` | **Logic**: Delegates user creation and hashing to the service. |
| **21** | `return Ok(result);` | Returns the JSON Web Token (JWT) on success. |
| **23-25** | `catch (InvalidOperationException ex)` | **Error**: Returns 400 if the email is already in use. |
| **29** | `[HttpPost("login")]` | **Action**: Public POST endpoint for returning users. |
| **30** | `Login(LoginRequestDto req, ...)` | Handlers credential validation. |
| **34** | `await _auth.LoginAsync(...)` | **Logic**: Verifies the password hash and generates a session token. |
| **37-39** | `catch (UnauthorizedAccessException ex)` | **Error**: Returns 401 if credentials do not match. |

## Program Flow
1. **Register**: Receives `RegisterRequestDto` -> Calls `_auth.RegisterAsync` -> Returns `AuthResponseDto`.
2. **Login**: Receives `LoginRequestDto` -> Calls `_auth.LoginAsync` -> Returns `AuthResponseDto`.
3. **Error Handling**: Wrapped in try-catch blocks to catch specific exceptions like `UnauthorizedAccessException` and return meaningful errors to the frontend.

## Connections
- **Depends on `IAuthService`**: For business logic and security.
- **Uses `AuthResponseDto`**: As the standard response format.

## Why it is used
It provides a public API for identity management. It is one of the few controllers that allows `AllowAnonymous` access (no token required to register or login).

## HTTP POST Methods: Detailed Explanation

### 1. `Register()`
- **Endpoint**: `POST /api/auth/register`
- **Working**: Handlers new user creation. It passes the request to the `AuthService`, which hashes the password, saves the user to the DB, and returns a JWT token so the user is logged in immediately.

### 2. `Login()`
- **Endpoint**: `POST /api/auth/login`
- **Working**: Authenticates returning users. It verifies the email and password hash. On success, it issues a new JWT token.

## Note on GET, PUT & DELETE
Authentication actions use **POST** exclusively to protect sensitive data like passwords. Updating user profiles or deleting accounts is handled by the `StudentsController` and `UsersController` respectively.

## Interview Questions
1. **Explain the benefits of keeping the Controller "Thin".**
   *Answer: By moving logic to a Service, the code becomes more testable and reusable. The controller's only job should be "Traffic Direction" (routing, input validation, and response formatting).*
2. **What is the purpose of the `CancellationToken` parameter?**
   *Answer: It allows the server to stop processing a request if the user cancels it (e.g., closes the browser tab), saving server resources.*
3. **Why return `BadRequest` for registration errors but `Unauthorized` for login errors?**
   *Answer: Consistency with HTTP standards. Registration usually fails due to "Bad Data" (e.g., email already exists), while login fails due to "Invalid Credentials" (unauthorized).*
