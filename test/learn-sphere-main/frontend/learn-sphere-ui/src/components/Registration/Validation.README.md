# Documentation: Validation.jsx (Frontend)

## Overview
`Validation.jsx` contains the "Rulebook" for all user input on the platform.

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **4** | `export const isEmail = ...` | **API**: High-performance boolean check for email string structure. |
| **10** | `if (email.length > 254)` | **Security**: Enforces global RFC length limits to prevent buffer or DB overflows. |
| **21** | `if (/\.\./.test(email))` | **Logic**: Prevents invalid consecutive dots in the email address. |
| **32** | `const localRegex = ...` | **Logic**: Whitelists only safe characters for the local part (before the @). |
| **36-37** | `const domainRegex = ...` | **Logic**: Complex regex that validates TLD length and label structure (no starting hyphens). |
| **45** | `export const normalizeEmail = ...` | **Logic**: Critical utility that forces the domain to lowercase for identity matching. |
| **56** | `export const passwordIssues = ...` | **API**: Returns a list of discrete security failures for a given password. |
| **62** | `if (value.length < 10)` | **Security**: Enforces a strict minimum length for account safety. |
| **63-67** | `!/[A-Z]/.test(value) ...` | **Security**: Series of regex checks for Uppercase, Lowercase, Number, and Special chars. |

## Why it is used
To ensure data integrity. By validating extensively on the frontend, we provide instant feedback to the user and prevent "Garbage" data from ever reaching our database.

## Interview Questions
1. **Why is your email validation more complex than a standard regex?**
   *Answer: Accuracy. Most regexes miss edge cases like consecutive dots (`..`) or invalid Top-Level Domains (TLDs). This function splits the email into local and domain parts to perform specific RFC-standard checks on each, such as ensuring the local part doesn't exceeds 64 characters.*
2. **What is "Email Normalization" and why is it important in `normalizeEmail`?**
   *Answer: It is the process of making strings consistent. Since email domains are case-insensitive, we lowercase only the domain part. This ensures that `user@GMAIL.com` and `user@gmail.com` are treated as the same unique user in our system.*
3. **How does the `passwordIssues` function return multiple errors?**
   *Answer: It initializes an empty array and pushes a specific string for every "Safety Rule" that is broken. This allows the UI to show a checklist of requirements to the user (e.g., "Missing uppercase", "Missing number") simultaneously.*
