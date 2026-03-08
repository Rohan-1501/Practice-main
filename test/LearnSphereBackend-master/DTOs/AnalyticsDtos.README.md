# Documentation: AnalyticsDtos.cs

## Overview
`AnalyticsDtos.cs` contains the data structures used to send summarized performance data to the Admin Dashboard.

## Detailed Explanation
These DTOs (Data Transfer Objects) are specialized classes that combine data from multiple tables (Courses, Students, Enrollments) into simplified summaries. They are "Read-Only" objects designed for efficient frontend rendering.

## Line-by-Line Explanation

| Line(s) | Explanation |
| :--- | :--- |
| **1** | Namespace declaration for the DTO layer. |
| **3** | `AnalyticsSummaryDto`: High-level dashboard stats (counts). |
| **5-10** | **Metric Properties**: Total counts for courses, enrollees, passes, failures, etc. |
| **13** | `StudentCourseDetailDto`: Specific enrollment summary for a student. |
| **15-22** | **Record Detail**: Combines identifiers, title, and outcome (grade, score, status). |
| **24** | `StudentPerformanceDto`: Master record for a student's analytics view. |
| **26-28** | **Student Identity**: Guid, Name, and Email for identification. |
| **30** | **Nested List**: Collection of all course details for THIS student. |
| **33** | `CoursePerformanceDto`: Master record for a course's analytics view. |
| **35-42** | **Course Metrics**: Metadata plus aggregate pass/fail counts. |
| **43** | **Nested DTO**: Link to the attendance breakdown. |
| **46** | `AttendanceStatsDto`: Simplified participation ratio. |
| **48-49** | **Logic**: Expected vs Actual attendance counts. |

## Program Flow
1. **Request**: The Admin opens the Analytics page on the frontend.
2. **Aggregration**: The `AnalyticsController` calculates totals (Total Courses, Success Rates, etc.).
3. **Packaging**: The data is mapped into an `AnalyticsSummaryDto` or a list of `StudentPerformanceDto`.
4. **Delivery**: The JSON is sent to the frontend to populate charts and tables.

## Why it is used
It prevents sending raw, heavy database entities over the network. It allows the backend to perform complex calculations (like "Total Passed") once and send only the result to the client.

## Interview Questions
1. **What is the difference between an Entity and a DTO?**
   *Answer: An Entity (like `Course.cs`) maps directly to a database table, while a DTO (like `AnalyticsSummaryDto`) is a custom shape designed specifically for API responses, often combining data from multiple entities.*
2. **Why use a `List<StudentCourseDetailDto>` inside `StudentPerformanceDto`?**
   *Answer: This allows for "Hierarchical Data" delivery. In one API call, the frontend gets the student's name AND a summary of every course they are in.*
3. **What is the benefit of the `AttendanceStatsDto`?**
   *Answer: It abstracts complex attendance logic into two simple numbers (`Enrolled` vs `Attended`), making it easy for the frontend to render a "Participation Rate" percentage.*
