# Documentation: http.js

## Overview
`http.js` is the centralized configuration for all network requests in the LearnSphere frontend.

## Detailed Explanation
Instead of calling `fetch()` directly everywhere, the app uses an **Axios Instance**. This allows us to define global rules for every request:
- **Base URL**: Every request automatically starts with `http://localhost:5267/api/`.
- **JWT Injection**: An "Interceptor" automatically pulls the student's token from `localStorage` and adds it to the `Authorization` header.
- **Form Data Handling**: A clever check sees if you are uploading a file (like a video) and ensures the browser handles the "Boundary" headers correctly.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Imports the `axios` library. |
| **3** | Sets the `API_BASE` constant pointing to the locally running C# backend. |
| **5-7** | **Instance Creation**: Creates a pre-configured Axios instance to share settings. |
| **10** | `http.interceptors.request.use`: Registers a function to run before every request leaves. |
| **11-12** | **Auth Logic**: Retrieves the JWT from `localStorage` and injects it into the `Authorization` header. |
| **16-18** | **Form Handling**: Removes manual `Content-Type` for `FormData` objects to allow the browser to auto-calculate boundaries. |
| **20** | Returns the modified config to continue the request. |
| **24-26** | `http.interceptors.response.use`: Registers a function to run when a response arrives. |
| **27-32** | **Logout Logic**: If the status is `401` (Unauthorized), it wipes the session and forces a login redirect. |
| **33** | Rejects the promise so the calling component can also handle the error. |

## Program Flow
1. **Request Interceptor**: Before any request leaves the browser, this function runs. It attaches the `Bearer` token if one exists.
2. **Response Interceptor**: When data comes back from the server, this function runs first. If the server says `401 Unauthorized` (the token expired), the app automatically logs the user out and sends them to the `/login` page.

## Why it is used
To ensure security and consistency. By centralizing the logic for tokens and URLs, we eliminate the risk of forgetting an authorization header on a single page, which would break the feature.

## Interview Questions
1. **What is an Axios Interceptor?**
   *Answer: It is a "Middleware" for your network requests. You can think of it like a security guard at a gate. In our case, the Request Interceptor "Stamps" Every outgoing request with a security token, and the Response Interceptor "Checks" every incoming package to see if the user is still allowed to be in the app.*
2. **Why do you delete the `Content-Type` header for `FormData`?**
   *Answer: Browser Intelligence. When uploading files, the browser needs to set a very specific `multipart/form-data` header that includes a unique "Boundary" string (to separate the different parts of the file). If we manually set it to just `multipart/form-data`, we would overwrite that boundary and the server wouldn't know how to read the file. Deleting it lets the browser generate the correct one automatically.*
3. **How does the global error handling work for 401 statuses?**
   *Answer: Centralized Recovery. Instead of adding `try/catch` with a redirect on every single page, the interceptor catches 401s once in `http.js`. It clears the `localStorage` and uses `window.location.href` to force the user back to the login screen, ensuring they can't continue using a broken session.*
  
