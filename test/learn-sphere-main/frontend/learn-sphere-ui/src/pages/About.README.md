# Documentation: About.jsx

## Overview
`About.jsx` is a simple, presentational page that explains the mission and vision of LearnSphere.

## Detailed Explanation
This page uses Vanilla CSS variables for theming and a clean, wide layout. It is a "Static" page, meaning it doesn't fetch any data from the server.

## Program Flow
- **Direct Render**: The component returns a collection of `<h1>`, `<h2>`, and `<p>` tags wrapped in a responsive container.

## Why it is used
To provide context to new users. It builds a brand identity by explaining the "Why" behind the platform (technical education accessibility).

## Interview Questions
1. **Explain the purpose of `mx-auto max-w-4xl` in the layout.**
   *Answer: Adaptive Typography. By limiting the width to `max-w-4xl` and centering it with `mx-auto`, we ensure that the lines of text aren't too long on large monitors. Long lines are harder to read, and this constraint keeps the text readable and professional.*
  
