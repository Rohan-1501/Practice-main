# Documentation: AssessmentPlayer.jsx

## Overview
`AssessmentPlayer.jsx` is the "Final Exam Engine". It is a high-stakes, timed component that determines if a student deserves a course certificate.

## Detailed Explanation
This component is the most logic-heavy part of the student experience. It manages:
- **Eligibility Validation**: It checks if the student has finished all lessons and passed all quizzes before even showing the "Start" button.
- **State Persistence**: If a student's browser crashes, the player can "Resume" an ongoing attempt by fetching the `startedAt` time from the server.
- **Multi-Modal Questions**: Supports both Single Choice (MCQ) and Multiple Select (check all that apply) formats.
- **Auto-Submission**: If the timer hits `00:00`, the component automatically stops the exam and sends the current answers to the server.

## Line-by-Line Explanation

## Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **1-14** | `import ... from 'useState'` | **UI**: Core React hooks and FontAwesome icon definitions. |
| **21** | `const [phase, setPhase] = ...` | **State**: Tracks the exam lifecycle (Locked -> Open -> Ongoing). |
| **26** | `const [timer, setTimer] = ...` | **State**: Reactive countdown display for the student. |
| **33-50** | `loadData = async () => { ... }` | **Async**: Fetches both eligibility and prior attempt history. |
| **57-64** | `resumeAttempt = async (...)` | **Logic**: Restores the exam state if the page is refreshed. |
| **75-81** | `beginSession = async (...)` | **Logic**: Commits the "Start" event to the backend database. |
| **85-98** | `setInterval(() => { ... })` | **Logic**: 1s heart-beat to reduce the clock until timeout. |
| **105-120** | `handleSubmit = async (...)` | **Logic**: Stops the timer and submits JSON answers to the API. |
| **124-139** | `handleSelect = (idx) => { ... }` | **UX**: Toggles or selects an answer for the current question. |
| **150-165** | `<div className="locked-phase">` | **UI**: The "Restricted Access" screen with progress requirements. |
| **210-230** | `<div className="eligible-phase">` | **UI**: The "Welcome" screen with instructions and start button. |
| **280-300** | `<div className="started-phase">` | **UI**: The live exam environment with the active question. |
| **400-450** | `<div className="result-phase">` | **UI**: The score recap and celebratory feedback animation. |
| **461-489** | `const ProgBar = ...` | **Sub**: Specialized progress bar for lockout requirements. |
| **491-523** | `const HistoryTable = ...` | **Sub**: Interactive table for viewing past certificate attempts. |

## Program Flow
1. **Lock Check**: On load, it checks the `eligibility` status. If locked, it shows a progress bar explaining exactly what is missing.
2. **The Timer Loop**: Once started, a `setInterval` runs every second, calculating the remaining time based on the server's `startedAt` time.
3. **The Paginator**: It shows one question at a time to prevent "Overlay" and keep the student focused.
4. **Result Processing**: Upon submission, it switches to a "Result" phase, showing the score, a pass/fail trophy, and a breakdown of correct answers.

## Why it is used
To provide a secure and professional testing environment. By handling timers and eligibility checks natively, it ensures the certification process is fair and reliable.

## Interview Questions
1. **How do you handle "State Resumption" if a user refreshes the page during an exam?**
   *Answer: Server-Side Sync. We don't rely only on local state. When the component mounts, it calls `getEligibility`. If there is an `ongoingAttemptId`, we call `assessmentApi.start` which returns the session data. We then calculate the elapsed time by comparing the server's `startedAt` timestamp with the current time.*
2. **Explain the logic in `handleSelect` for `MultipleSelect` questions.**
   *Answer: Toggle Logic. For multi-select, we treat the answer as an array. If a user clicks an option that is already in the array, we `.filter()` it out (remove it). If it's not there, we use the "Spread Operator" (`[...cur, idx]`) to add it.*
3. **Why do you use `useRef` for the timer instead of just `useState`?**
   *Answer: Cleanup and Direct Reference. `timerRef.current` holds the interval ID. This allows us to clear the timer from any function (like `handleSubmit`) or during the component "Unmount" phase, preventing memory leaks and ensuring the timer stops the moment the exam is over.*
4. **What is `useCallback` and why is it useful?**
   *Answer: It is used for "Memoization". It caches a function definition so it isn't recreated on every render. This is important when passing functions to optimized child components to prevent unnecessary re-renders.*
5. **Why use local state or Context instead of Redux?**
   *Answer: Redux is like a "Power Plant"—it's great for huge cities (vast, interconnected state), but overkill for a single house (a modular component like this). Context/local state keep the app faster and easier to maintain for our current scale.*
  
