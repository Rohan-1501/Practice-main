# Documentation: Navbar.jsx

## 🌐 Overview
The `Navbar` is the central "Command Center" of the LearnSphere frontend. It is a sticky, glassmorphic component that stays at the top of the UI to provide persistent navigation and real-time feedback.

## 🛠️ Detailed Explanation
The Navbar isn't just a list of links; it is a **reactive** component that manages several background tasks:
- **State Synchronization** [L:66-80]: It "listens" to the browser's storage. If a user logs in via a modal or another tab, the Navbar updates immediately without a page refresh.
- **Smart Notification Polling** [L:43-48]: It runs an active 1-second interval to keep the unread count accurate.
- **Performance Optimization** [L:96-109]: It automatically pauses background tasks (like polling) when the component is unmounted or when the user switches to a different browser tab.

## 🔄 Program Flow
1.  **Initial Load** [L:9-15]: Reads the `learnsphere_user` from `localStorage` to set the initial `user` state.
2.  **The Poller** [L:83-93]: If the user is a `student`, it triggers `startPolling()`, which calls the backend every **1,000ms**.
3.  **Visibility Guard** [L:96-109]: Using `visibilitychange` and `focus` events, it pauses the poller when the tab is inactive and resumes with an immediate refresh when the user returns.
4.  **Event Bus** [L:112-130]: It listens for custom events:
    - `userUpdated` [L:74]: Syncs login/logout states.
    - `notificationsUpdated` [L:123]: Allows other components (like a notification list) to tell the Navbar to refresh the badge count.
5.  **Rendering** [L:141-217]: Uses role-based logic to show different action buttons (e.g., `Sign up` [L:201-206] for guests vs. `Profile` [L:182-197] for students).

## � Line-by-Line Explanation

| Line | Code Snippet | Explanation |
| :--- | :--- | :--- |
| **9-15** | `const [user, setUser] = useState(...)` | **State**: Initializes the logged-in user from `localStorage` immediately on load. |
| **24-41** | `const fetchUnread = useCallback(...)` | **Logic**: The core function that fetches notification counts with a "Race Condition" safety check [L:37]. |
| **47** | `setInterval(fetchUnread, 1000);` | **Timer**: Triggers a background check every 1 second to keep the UI real-time. |
| **60-63** | `mountedRef.current = false;` | **Safety**: Ensures no state updates are attempted after the user navigates away from the page. |
| **74-75** | `window.addEventListener(...)` | **Sync**: Watches for login/logout events happening in other parts of the app or other tabs. |
| **97-103** | `window.addEventListener("focus", ...)` | **Optimization**: Refreshes the count as soon as the user returns to the tab. |
| **123-124** | `window.addEventListener("notificationsUpdated", ...)` | **Event Bus**: Allows other components (like a "Mark as Read" button) to force a Navbar refresh. |
| **182-197** | `<Link to={user.role === "admin" ...}>` | **UI**: Dynamically changes the destination of the Profile button based on the user's role. |

## �💎 Design Notes
- **Glassmorphism** [L:142]: Uses `backdrop-blur-md` and `bg-black/40` for a modern, semi-transparent look.
- **Dynamic Badge** [L:171-178]: The red notification circle features `aria-live="polite"` to ensure screen readers announce new notification counts.

## 🎓 Study Questions
1.  **Why use `useCallback` for `fetchUnread`?** [L:24]
    *Because it is used as a dependency in multiple `useEffect` hooks and the `setInterval`. Without it, the function would be re-created on every render, causing the poller to reset unnecessarily.*
2.  **How does it handle "Admin" users differently?** [L:25, L:84]
    *Admins do not have a notification bell. The polling logic includes a check—if the user is an admin, the poller is stopped, and the count is forced to 0.*
3.  **What is the purpose of `inFlightStampRef`?** [L:20, L:37]
    *It prevents "Race Conditions". If a network request takes 5 seconds to finish, but the user logs out after 2 seconds, the `inFlightStampRef` ensures the old response doesn't accidentally overwrite the "logged out" state.*
