# Documentation: App.css

## Overview
`App.css` defines the "Global Design System" for the LearnSphere application.

## Detailed Explanation
Instead of hardcoding colors in every component, this file uses **CSS Custom Properties** (Variables). This makes the app incredibly easy to theme.
- **Root Variables**: Defines the "Light Mode" defaults (creamy white backgrounds, dark slate text).
- **Dark Mode Support**: Uses the `[data-theme="dark"]` selector to swap variables for a premium dark experience (gradients and high-contrast text).
- **Global Animations**: Defines the `marquee` utility used for the scrolling news bar on the landing page.

## Program Flow
- **Inclusion**: Imported in `main.jsx`, meaning these styles are available to every single component in the project.

## Why it is used
For visual consistency. By setting a base background and text color at the root, we ensure that if a component doesn't specify a color, it still looks like it belongs to the platform.

## Interview Questions
1. **What is the advantage of using CSS Variables over hardcoded hex codes?**
   *Answer: Maintainability. If a designer decides to change the "Primary Indigo" color, we only have to change it in one line in `App.css` instead of searching and replacing it in 50 different files.*
2. **Explain the `radial-gradient` used in the dark theme.**
   *Answer: Depth and Premium Feel. Instead of a flat black background, the radial gradient creates a "Pulse" of color from the corner. This mimics professional software designs and makes the UI feel more modern and high-end.*
  
