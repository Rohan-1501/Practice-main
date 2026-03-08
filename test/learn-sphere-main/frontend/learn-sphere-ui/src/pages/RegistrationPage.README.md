# Documentation: RegistrationPage.jsx

## Overview
`RegistrationPage.jsx` is the entry point for new students to join the LearnSphere platform.

## Detailed Explanation
This page holds the `RegistrationForm`. It is a simple, focused page designed to guide the user through the account creation process.

## Program Flow
1. **Form Integration**: It passes a `onRegistered` callback to the `RegistrationForm` component.
2. **Post-Registration Hand-off**: Once the form successfully creates the user, this page takes over and navigates the student to the `/dashboard`.

## Why it is used
To provide a clean, dedicated space for conversion. By keeping the registration process separate from the main app, it ensures the user isn't distracted while filling out their important account details.

## Interview Questions
1. **Why is the layout for the registration page so thin (maxWidth: 480)?**
   *Answer: Focus and Readability. Registration forms become intimidating when they are too wide. By restricting the width, we keep the inputs stacked vertically in a way that is easy for the eye to scan.*
2. **Explain the benefits of the `useNavigate` hook here.**
   *Answer: Non-Blocking Flow. Instead of using a traditional `<a href="...">` which causes a full browser reload, `useNavigate` allows us to switch pages instantly within the React memory, keeping the user's session active.*
  
