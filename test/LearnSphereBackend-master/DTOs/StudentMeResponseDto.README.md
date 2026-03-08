# Documentation: StudentMeResponseDto.cs

## Overview
`StudentMeResponseDto.cs` provides a detailed profile of the currently logged-in student.

## Detailed Explanation
The "Me" in the name indicates that this DTO is returned when a student requests their own profile details. It includes personal, academic, and guardian info, as well as a formatted `RegistrationDate`.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Namespace declaration. |
| **3** | `StudentMeResponseDto`: Profile view tailored for the current student. |
| **5** | `FullName`: User's combined descriptive name. |
| **6-7** | **Demographics**: Optional birthday (DateOnly) and gender string. |
| **8** | `Email`: Primary contact address. |
| **9-10** | **Contact**: Country location and phone number. |
| **12-14** | **Academic**: Student identifier, current course, and study year. |
| **16-19** | **Guardian**: Emergency contact name, phone, email, and address. |
| **20** | `RegistrationDate`: Backend-formatted string for easy UI display. |

## Interview Questions
1. **Why is `RegistrationDate` a string here but `CreatedAt` is a DateTime in the model?**
   *Answer: To provide a pre-formatted, user-friendly date (e.g., "March 7th, 2026") directly from the backend, reducing the formatting work needed on the frontend.*
2. **What is the purpose of including `DateOfBirth` as a `DateOnly?`?**
   *Answer: To ensure that only the date is transmitted, avoiding unnecessary time data and potential "off-by-one" errors caused by timezones.*
