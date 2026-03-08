# Documentation: VisualEditor.jsx

## Overview
`VisualEditor.jsx` is a custom-built rich text editor for course creators.

## Detailed Explanation
It provides a "What You See Is What You Get" (WYSIWYG) experience. Instead of typing Markdown code, admins can click buttons to bold text, create lists, or insert links. It uses the browser's native `contentEditable` API and `document.execCommand` for text manipulation.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **9** | `const editorRef = useRef(null);` | **DOM**: A persistent reference to the `contentEditable` div for command manipulation. |
| **14-21** | `useEffect(() => { ... })` | **Sync**: Hydrates the editor with initial content while preventing infinite update loops. |
| **23-29** | `handleInput = () => { ... }` | **Event**: Reads the innerHTML of the div and emits it to the parent state. |
| **31-41** | `handlePaste = (e) => { ... }` | **UX**: Intercepts the clipboard to ensure the editor handles HTML/Plaintext correctly. |
| **43-47** | `const exec = (cmd, val) => ...` | **Core**: Wrapper for `document.execCommand` that manages focus and state sync. |
| **56-91** | `<button ... onClick={() => exec("bold")} ... />` | **UI**: Toolbar buttons that trigger the native browser document editing engine. |
| **96-119** | `exec("formatBlock", "h1")` | **Logic**: Converts the current line into a semantic heading element. |
| **143-151** | `prompt("Enter URL:")` | **UX**: Simple interactive prompt to capture link targets for the selection. |
| **182-194** | `contentEditable` | **React**: Marks the div as a rich-text surface; the heart of the "What You See Is What You Get" engine. |
| **196-302** | `<style>{ ... }</style>` | **UI**: Scoped CSS that ensures the editor looks identical to the final rendered output. |

## Why it is used
To make course creation accessible to non-technical instructors who might find raw HTML or Markdown confusing.

## Interview Questions
1. **Why use `contentEditable` instead of a standard `<textarea>`?**
   *Answer: A `<textarea>` can only display plain text. `contentEditable` allows the browser to render HTML (bold, colors, images) directly inside the input area.*
2. **What is the `dangerouslySetInnerHTML` prop and why is it used here?**
   *Answer: React normally escapes HTML to prevent XSS attacks. We use this prop to deliberately "Inject" the current HTML value into the editor when it first loads.*
3. **How do you sync the editor content with React state?**
   *Answer: We use a `ref` on the editable div. When the `input` event fires, we read `ref.current.innerHTML` and trigger the `onChange` callback.*
