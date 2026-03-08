# Documentation: SafeMarkdown.jsx

## Overview
`SafeMarkdown.jsx` is a lightweight, custom-built renderer that converts Markdown text into styled HTML.

## Detailed Explanation
Instead of using a heavy library like `react-markdown`, this project uses a custom `renderMarkdown` function. It uses a series of Regular Expressions (Regex) to find Markdown patterns (like `**bold**`) and replace them with standard HTML tags (like `<strong>bold</strong>`). It also includes a basic HTML escape to prevent simple XSS attacks.

## Program Flow
1. **Sanitization**: Replaces `<`, `>`, and `&` with safe HTML entities.
2. **Regex Pipeline**:
   - Finds `# Headings` and converts to `<h1>`.
   - Finds `[links](url)` and converts to `<a>` tags with `rel="noopener noreferrer"`.
   - Converts newlines (`\n`) into `<br />`.
3. **Injection**: Uses `dangerouslySetInnerHTML` to render the resulting HTML string.

## Why it is used
To provide a fast and secure way to render course descriptions and student messages without adding extra bundle size.

## Interview Questions
1. **Why use regex instead of a parsing library?**
   *Answer: Bundle size and specific control. For simple bold/italic/link formatting, a few regex lines are much "Lighter" than a full markdown-it or remark parser. It makes the page load faster.*
2. **How does this component prevent basic XSS?**
   *Answer: The first three `.replace()` calls escape the `<` and `>` characters. This means if a user types `<script>`, it gets turned into `&lt;script&gt;`, which the browser renders as text instead of executing as code.*
3. **What is the risk of using `dangerouslySetInnerHTML`?**
   *Answer: It bypasses React's security by injecting a raw string into the DOM. This is why the sanitization step is critical—without it, an attacker could steal user cookies by injecting malicious scripts.*
