# Documentation: App.jsx

## Overview
`App.jsx` is the "Air Traffic Controller" of LearnSphere, directing users to the correct page based on the URL.

## Detailed Explanation
This component defines the entire **Route Map** of the application. It uses a declarative approach to handle:
- **Public Access**: Pages like Home, About, and Login that anyone can see.
- **Student Privacy**: Pages like the Dashboard and Course Player that are wrapped in `ProtectedRoute`.
- **Admin Authority**: Advanced management tools wrapped in `ProtectedAdminRoute`.
- **Layout Consistency**: It ensures that the `Navbar` and `Footer` are always visible regardless of which page the user is viewing.

## Program Flow
1. **Shell Rendering**: Renders the `Navbar`.
2. **Main Container**: Renders a `<main>` tag with a specific `pb-[64px]` (calculated to match the footer height) so content never disappears behind the bottom bar.
3. **Route Matching**: The `<Routes>` component looks at the current browser URL (e.g., `/admin/users`) and renders the corresponding component (e.g., `UsersAdmin`).

## Why it is used
To provide a Single Page Application (SPA) experience. Instead of the browser requesting a new file from the server every time a user clicks a link, `App.jsx` simply swaps out the middle section of the screen, making the app feel instantaneous.

## Interview Questions
1. **How do the `ProtectedRoute` and `ProtectedAdminRoute` work?**
   *Answer: Component Composition (Higher Order Patterns). These components act as "Wrappers". They check the user's login state and role before allowing the `children` (the actual page) to render. If the user isn't allowed, they are automatically redirected.*
2. **Explain the use of `:slug` and `:id` in the routes.**
   *Answer: Dynamic Routing. Instead of creating a separate route for every single course, we use a "Placeholder". When a user visits `/course/javascript-basics`, React Router captures `javascript-basics` as the "slug", allowing the page to fetch the correct data for that specific course.*
3. **Why is the `<Footer />` placed outside of the `<Routes />`?**
   *Answer: Persistent UI. By placing it outside, we ensure that the footer remains mounted and visible during transitions, preventing it from "Flickering" or disappearing as the user navigates between pages.*
4. **When would you use `useRef` instead of `useState`?**
   *Answer: Use `useRef` when you need to store a value across renders but **DO NOT** want to trigger a re-render when the value changes (e.g., storing a timer ID or accessing a DOM element directly). `useState` is used for data that actually affects what is shown on the screen.*
5. **What is `useCallback` and why is it useful?**
   *Answer: It is used for "Memoization". It caches a function definition so it isn't recreated on every render. This is important when passing functions to optimized child components to prevent unnecessary re-renders.*
6. **Why did you use React Context (or local state) instead of Redux here?**
   *Answer: "Right Tool for the Job". For an application of this scale, the built-in React Context API (used in our `AuthContext`) and local state are more than sufficient. Redux adds significant boilerplate and complexity. Redux is better suited for extremely large apps with complex, global state that changes frequently across many disconnected components.*
  
