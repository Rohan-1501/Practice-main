# Documentation: Profile.jsx

## Overview
`Profile.jsx` is the "Identity Management" page where students can view their academic status and edit their personal information.

## Detailed Explanation
This page serves two roles:
1. **Presentation**: A clean view of the student's name, course, year, and registration date.
2. **Configuration**: A tabbed editor (PersonalInfo, PAcademic, GuardianInfo) that allows the student to update their records.

It is heavily integrated with `localStorage` to ensure that data is preserved even if the user accidentally closes the tab while editing.

## Program Flow
1. **Initial Hydration**: On mount, it fetches the latest data from `getMeApi`. It then immediately populates `localStorage` with these values to ensure all sub-components are "Hydrated" with the latest server data.
2. **Custom Events**: It listens for a global `profileSaved` event. This allows the sub-components (which actually do the saving) to tell the main page to "Exit Edit Mode" automatically when the work is done.
3. **Logout Logic**: Triggers a full cleanup: removes tokens, clears user data, dispatches a `userUpdated` event to fix the Navbar, and reloads the page to clear the React state.

## Why it is used
To provide a sense of "Ownership". By allowing students to maintain their records and see their academic "Stats", it creates a professional and reliable learning profile.

## Interview Questions
1. **Why do we use `window.dispatchEvent` for the Logout and Save functions?**
   *Answer: Cross-Component Communication. The `Profile` page and the `Navbar` are in different parts of the application tree. By sending a custom event like `userUpdated`, the Profile page can tell the Navbar to refresh the user's name without needing a complex global state library like Redux.*
2. **Explain the purpose of the `registrationDate` logic.**
   *Answer: Data Robustness. Because different backend versions might send the field as `registrationDate` or `RegistrationDate`, we check both. We also "Cache" it in `localStorage` so the profile shows the date instantly on subsequent loads even before the API call finishes.*
3. **How does the `editMode` state control the sub-components?**
   *Answer: Conditional Rendering. We use a simple boolean. When `editMode` is true, we hide the "Stats" boxes and show the three complex form components. This prevents the screen from becoming too cluttered and keeps the user focused on the task at hand.*
  
