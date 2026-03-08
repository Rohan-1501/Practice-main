# Documentation: LoginRequestDto.cs

## Overview
`LoginRequestDto.cs` is a simple object used to transmit a user's login credentials.

## Detailed Explanation
It strictly requires an `Email` and a `Password`. This is the very first object sent when a user tries to access the platform.

## Why it is used
It isolates the credentials from the rest of the user's profile.

## Interview Questions
1. **Should you use `GET` or `POST` when sending this DTO to the server?**
   *Answer: Always `POST`. `GET` requests put the data in the URL, which is a major security risk as it gets saved in browser history and server logs. `POST` sends it in the request body.*
2. **Why is `Password` an empty string by default?**
   *Answer: To prevent null reference exceptions in the controller if the user sends an empty JSON object `{}`.*
