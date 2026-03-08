# Documentation: PersonalInfo.jsx

## Overview
`PersonalInfo.jsx` is the first part of the student profile editor, focusing on core identity.

## Detailed Explanation
This component creates a "Seamless" editing experience. It doesn't have a "Save" button for individual fields. Instead, it uses `useEffect` to watch every input (Name, Email, DOB) and automatically save the value to `localStorage` as soon as the student stops typing.

## Program Flow
1. **Init**: On mount, it reads all values from `localStorage` to populate the inputs.
2. **Auto-Save**: A `useEffect` hook runs whenever the state changes, writing the new values back to `localStorage`.
3. **Password Security**: It uses the `InputField` component with `type="password"` to ensure the student's password remains hidden while they update it.

## Why it is used
To prevent data loss. If a student's internet drops or their battery dies while filling out their profile, their progress is safely stored in their browser and will be there when they return.

## Interview Questions
1. **Explain the benefits of "Auto-Saving" to `localStorage` in this context.**
   *Answer: User experience and resilience. It removes the stress of having to click "Save" constantly and ensures that the multi-step profile process (Personal -> Academic -> Guardian) feels connected even though they are separate components.*
2. **What are the privacy implications of storing a password in `localStorage`?**
   *Answer: It is generally a medium-risk practice. While convenient for this specific platform's draft system, in a high-security banking app, you would never store passwords in plain text in the browser. For an educational app, it's a trade-off for usability.*
3. **Why do we use TWO `useEffect` hooks here?**
   *Answer: Separation of Concerns. One hook handles "Loading" (running only once at the start). The other handles "Saving" (running every time data changes). This keeps the logic organized and prevents infinite loops.*
