# Documentation: RegisterRequestDto.cs

## Overview
`RegisterRequestDto.cs` is used when a new user signs up for the platform.

## Detailed Explanation
It contains the basic trio of `Name`, `Email`, and `Password`. The backend uses this to create both a `User` record and an initial (empty) `Student` profile.

## Interview Questions
1. **What validation would you normally perform on this DTO?**
   *Answer: Email format validation, password strength requirements (min length, special characters), and ensuring the Name isn't just whitespace.*
2. **Why is this a separate class from `LoginRequestDto`?**
   *Answer: Because registration often requires more fields (like `Name`) than login, and keeping them separate prevents confusion in the controller logic.*
