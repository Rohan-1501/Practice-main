# Documentation: GuardianInfo.jsx

## Overview
`GuardianInfo.jsx` is the final step and the "Aggregator" of the student profile.

## Detailed Explanation
This component has two major responsibilities:
1. **Guardian Data Entry**: Collecting parent/guardian contact details.
2. **Final Submission**: It acts as the "Submit Button" for the entire profile. When the user clicks "Submit", this component pulls all the pieces of data (Personal, Academic, and Guardian) from `localStorage` and sends them to the server in one large API request.

## Program Flow
1. **Drafting**: The "Save Draft" button explicitly saves the guardian-specific data to a `guardian_draft` key.
2. **Aggregation**: On "Submit", it constructs a `payload` object by reading 13 different keys from `localStorage`.
3. **API Call**: Invokes `saveMeApi(payload)` to persist the complete profile in the SQL database.
4. **Feedback**: Uses `react-toastify` to show a "Success" or "Error" notification at the top-right of the screen.

## Why it is used
It completes the user's registration journey. Once this form is submitted, the student's profile moves from "Basic" to "Verified", allowing for deeper integration with platform features.

## Interview Questions
1. **Explain the purpose of `window.dispatchEvent(new CustomEvent("profileSaved"))`.**
   *Answer: Inter-component communication. Other parts of the dashboard might need to refresh their data once the profile is updated. By firing this event, any component listening for "profileSaved" can trigger a fresh fetch.*
2. **How does this component handle "Null" values during submission?**
   *Answer: It uses the logical OR operator (`|| null`). If a value is missing in `localStorage`, it sends `null` to the backend. This ensures the backend receiving the JSON knows exactly which fields were left blank.*
3. **What is the purpose of the `setTimeout` before the event dispatch?**
   *Answer: To ensure the "Success" toast has enough time to be seen by the user before the page potentially triggers a reload or a heavy data refresh.*
