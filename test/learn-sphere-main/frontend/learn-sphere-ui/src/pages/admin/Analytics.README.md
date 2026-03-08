# Documentation: Analytics.jsx

## Overview
`Analytics.jsx` is the reporting engine that helps admins track student success and course popularity.

## Detailed Explanation
This page acts as a "Business Intelligence" dashboard. It aggregates data into two primary reports:
- **Student Report**: Shows a list of students and their individual performance (grades, scores, and compliance status).
- **Course Report**: Shows which courses are the most popular and what the pass/fail rates look like.
It also features a **CSV Export** tool, allowing admins to download these reports and open them in Excel or Google Sheets.

## Program Flow
1. **Aggregation**: Fetches summary statistics (Total Enrolled, Passed, etc.) and detailed performance data in parallel.
2. **Expandable Rows**: In the student report, only the names are shown initially. Clicking a row expands it to show the student's full enrollment history (nested table).
3. **Pagination**: To keep the interface fast, it only displays 5 items at a time, providing "Next" and "Previous" buttons to navigate the data.

## Why it is used
To identify "At-Risk" students and "Underperforming" courses. If the analytics show a high failure rate for a specific course, admins can intervene to improve the content.

## Interview Questions
1. **How is the CSV Export implemented without a backend?**
   *Answer: Using the "Blob" and "Object URL" pattern. We convert the JSON data into a comma-separated string, wrap it in a `Blob` (Binary Large Object), create a temporary download link, and programmatically "Click" it for the user.*
2. **Why use `useMemo` for the search query and pagination?**
   *Answer: React efficiency. Searching through thousands of rows of data is a heavy operation. `useMemo` ensures that the search is only performed when the `searchQuery` or the `rawData` changes, keeping the typing experience smooth.*
3. **What is the purpose of the `compliance` field?**
   *Answer: It is a "Status Flag". In educational platforms, a student is "Compliant" if they have completed all mandatory checks and passed the assessment. This allows admins to filter for students who are ready for graduation.*
  
