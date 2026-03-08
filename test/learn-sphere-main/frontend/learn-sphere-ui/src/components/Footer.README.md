# Documentation: Footer.jsx

## 🌐 Overview
The `Footer` is a presentational component that provides a consistent conclusion to every page. It includes essential navigation links and dynamic metadata like the current year.

## 🛠️ Detailed Explanation
- **Dynamic Year** [L:4]: Instead of hardcoding the copyright year, it uses the `Date` object to ensure it stays current automatically.
- **Theming** [L:10-12]: Uses CSS variables (`--border`, `--card`, `--text`) to adapt its colors based on the global theme.
- **Responsive Layout** [L:16]: Switches from a column layout on mobile to a row layout on larger screens using Tailwind's `flex-col md:flex-row`.

## 🔄 Program Flow
1.  **Rendering**: The component calculates the current year [L:4].
2.  **Navigation**: It displays a set of static links [L:19-20] for high-level site information.

## 📊 Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **4** | `const year = new Date().getFullYear();` | **Logic**: Automatically retrieves the 4-digit year for the copyright notice. |
| **16** | `flex-col md:flex-row` | **CSS**: Ensures the footer looks good on phones (vertical) and desktops (horizontal). |
| **19-20** | `<a href="...">` | **Links**: Provides basic SEO-friendly navigation for static pages. |

## 🎓 Study Questions
1.  **Why use `new Date().getFullYear()` instead of hardcoding 2024?**
    *Answer: Automation. It ensures the copyright year is always accurate without requiring a manual code update every January 1st.*
2.  **Explain the `text-[var(--text)]/70` syntax used in the Tailwind classes.**
    *Answer: This is Tailwind's opacity shorthand. It tells the browser to use the color defined in the `--text` variable but with 70% opacity.*
