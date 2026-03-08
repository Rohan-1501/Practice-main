# Documentation: Contact.jsx

## Overview
`Contact.jsx` is a standard support page with a functional contact form.

## Detailed Explanation
This page allows users to send questions or feedback. It features a controlled form where React manages the state of the inputs before submission.

## Program Flow
1. **Local State**: Uses a `status` state to show a "Thank you" message after submission.
2. **Mock Submission**: Currently, the `onSubmit` function prevents the browser from reloading (via `e.preventDefault()`) and mocks a successful send without calling a real backend endpoint yet.

## Why it is used
To capture user feedback. It provides a direct channel for students to reach out to the LearnSphere team if they encounter issues or have suggestions.

## Interview Questions
1. **Why do we use `e.preventDefault()` in the `onSubmit` handler?**
   *Answer: Single Page Application (SPA) Behavior. By default, forms try to send data via a POST request and reload the page. In React, we want to stay on the same page and handle the data ourselves (usually via an API call). `preventDefault()` stops that default reload.*
2. **What is a "Controlled Component" in this context?**
   *Answer: An input whose value is driven by React state. Although this specific code uses standard HTML inputs, a fully controlled version would use `value={name}` and `onChange={e => setName(e.target.value)}` to ensure React is the "Single Source of Truth" for the form data.*
  
