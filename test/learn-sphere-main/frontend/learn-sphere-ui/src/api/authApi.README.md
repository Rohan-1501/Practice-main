# Documentation: authApi.js

## Overview
`authApi.js` is the specialized data service for user identity and authentication.

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1** | `import { http } from "./http";` | **Network**: Imports the centralized axios instance with pre-configured base URLs and interceptors. |
| **3** | `export const loginApi = ...` | **API**: Exports a POST request handler for the `/auth/login` endpoint. |
| **4** | `export const registerApi = ...` | **API**: Exports a POST request handler for the `/auth/register` endpoint. |

## Why it is used
To decouple authentication networking from the UI. Components like `LoginPage` don't need to know the specific URL or HTTP method; they simply call `loginApi(payload)`.

## Interview Questions
1. **Explain the benefits of centralizing API calls in files like `authApi.js`.**
   *Answer: Maintainability. If the backend endpoint changes from `/auth/login` to `/api/v2/auth/signin`, we only need to update it in one line here, rather than searching through every component.*
2. **What is a "Payload" in the context of these functions?**
   *Answer: It is the data bundle (the object containing email and password) that is serialized into JSON and sent in the body of the HTTP POST request.*
