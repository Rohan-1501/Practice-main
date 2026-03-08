# Documentation: AdminSidebar.jsx

## Overview
`AdminSidebar.jsx` is the primary control panel for administrators.

## Detailed Explanation
It provides a persistent sidebar for desktop users and a swipe-out drawer for mobile users. It manages:
- **Navigation**: Links to Analytics, Course Management, and User Management.
- **Theme Toggling**: Switches the entire application between "Dark" and "Light" modes by updating a `data-theme` attribute on the root HTML element.
- **State Persistence**: Ensures the chosen theme is remembered in `localStorage`.
- **Responsive Logic**: Hijacks the body scroll when the mobile menu is open to prevent a "Double Scroll" effect.

## Program Flow
1. **Init**: Reads the saved `theme` from `localStorage`.
2. **Toggle**: When the Sun/Moon icon is clicked, it updates the `documentElement` and saves the new preference.
3. **Logout**: Clears the `learnsphere_user` key and triggers a `userUpdated` event to notify other components (like the Navbar).

## Why it is used
To give admins a focused, separate environment from the student view. It uses `NavLink` to automatically highlight which management page is currently active.

## Interview Questions
1. **How is the "Theme Change" broadcast to the rest of the application?**
   *Answer: By setting an attribute on `document.documentElement`. CSS variables throughout the app (like `var(--bg)`) change their values based on whether that attribute is "light" or "dark".*
2. **Why do we use a separate `SidebarContent` sub-component inside the file?**
   *Answer: To follow the DRY (Don't Repeat Yourself) principle. Both the desktop sidebar and the mobile drawer use the exact same list of links. Defining them once in a sub-component makes the code easier to maintain.*
3. **What is the purpose of `window.dispatchEvent(new Event("userUpdated"))`?**
   *Answer: It is a way to communicate between components that aren't parent-child. When the sidebar logs the user out, the Navbar (which is a separate component) needs to know instantly so it can hide the profile icon. This custom event provides that "Signal".*
