# Project Challenges & Solutions

Every data project presents its own unique set of challenges. This document outlines the key hurdles encountered during the development of the Kaya Hotel Group dashboard and the solutions implemented to overcome them. This process highlights a commitment to best practices, problem-solving, and continuous learning.

## Challenge 1: Static Measure Calculation

### Problem
The Occupancy Rate KPI was showing the same value for every single month in a trend chart. Consequently, the Month-over-Month change was always 0%. This indicated that the date filter from the visual was not being applied to the measure correctly.

### Solution
The DAX formula itself was correct. The root cause was identified in the data model: there was no active relationship between the dim_date (our date table) and the fact_aggregated_bookings table (where the occupancy data resides).

By creating a relationship in the Model view between dim_date[date] and fact_aggregated_bookings[check_in_date], we enabled the filter context from the date dimension to flow to the fact table. This allowed the Occupancy Rate measure to be calculated correctly for each distinct month.

### Key Takeaway
A DAX measure is only as powerful as the data model it operates on. Correct and active relationships are fundamental for measures to calculate correctly against different dimensions.

## Challenge 2: Inconsistent and Inefficient Visual Styling

### Problem
Styling the KPI cards consistently was proving difficult. The Power BI Format Painter and the Copy/Paste functions were not reliably copying all the detailed formatting properties (like font sizes for the Callout and Reference labels), forcing the prospect of tedious, manual formatting for each visual.

### Solution
Instead of relying on the UI, we adopted a more robust, code-based approach by creating a JSON Theme file. This file programmatically defined the default styles for all "Card" visuals, including specific font families, sizes, and colors for the category labels, callout values, and reference labels. By importing this single theme file, all cards were instantly and perfectly formatted, ensuring absolute consistency.

### Key Takeaway
For professional-grade reports, programmatic styling using theme files is superior to manual formatting. It ensures consistency, saves significant time, and makes the report's design scalable and easy to update.

## Challenge 3: Debugging the JSON Theme File

### Problem
The initial versions of the kaya-theme.json file were rejected by Power BI upon import, showing errors related to "invalid JSON syntax" and later, "schema" mismatches (e.g., expecting an array [] but receiving an object {}).

### Solution
We adopted an iterative debugging process. By carefully reading the specific error messages from Power BI, we were able to:

Correct the initial syntax errors (like misplaced commas or braces).

Adjust the structure to match Power BI's required schema (changing objects back to arrays where required).
This methodical approach allowed us to pinpoint the exact issues and produce a valid and functional theme file.

### Key Takeaway
Error messages are not failures; they are instructions. Methodical debugging by reading and interpreting these messages is a core development skill.

## Challenge 4: Choosing the Correct Visual for KPIs

### Problem
The classic "KPI" visual was unintuitive. Its "Trend axis" field didn't directly accept a measure to display a percentage change prominently, making it impossible to build the dashboard according to the approved wireframe.

### Solution
We identified the limitations of the older visual and pivoted to the modern, more flexible "Card" visual. We leveraged its Reference labels feature in the formatting pane to add our [... MoM %] measures as a secondary metric. This gave us full control over the layout and formatting, perfectly matching our design.

### Key Takeaway
It's crucial to stay current with the features of your tools. A problem that is difficult or impossible with an older feature can often be solved elegantly and easily with a more modern one.

## Challenge 5: Inefficient Creation of DAX Measures

### Problem
The prospect of creating dozens of DAX measures one-by-one in the Power BI user interface was identified as a repetitive and inefficient workflow, especially for large-scale projects.

### Solution
We discussed the professional best practice of using external tools to solve this. Tabular Editor was identified as the industry-standard tool that allows developers to script and manage measures in bulk using C#. This enables faster development, better organization, and the ability to use version control for DAX logic.

### Key Takeaway
To scale from a report builder to an enterprise-level developer, it is essential to look beyond the basic user interface and embrace external tools that enhance efficiency, automation, and governance.
