# Retail Store:  Dynamic Executive Insight Dashboard

## Project Overview
This Power BI project provides an automated, dynamic storytelling experience for the executive management team at **IFEXA Retail** (a technology company selling laptops, phones, accessories, and networking equipment). 

Instead of just showing raw numbers, this dashboard interprets performance changes using advanced **DAX measures** to generate automated, context-aware business insights that shift based on user slicers.

##  Business Problem Solved
Traditional static dashboards require executives to click through multiple filters, look at different charts, and manually calculate performance metrics to understand corporate health. This manual process causes **delayed decision-making** and introduces human error in data interpretation.

By engineering this **Dynamic Executive Insight Dashboard**, I solved these core operational challenges:
*   **Eliminated Dashboard Fatigue:** Management no longer needs to hunt for insights. The system automatically reads the data charts and prints out a clear plain-text narrative summary.
*   **Real-Time Performance Tracking:** Implemented Month-over-Month (MoM) logic that tracks structural revenue swings automatically, flagging critical issues instantly.
*   **Dynamic Granular Drill-Downs:** The text narrative stays 100% synchronized with global slicers, translating abstract metrics instantly when drilling down into specific states or categories.

*  ##  DAX Measures & Business Logic

Below are the key calculations built into the model to satisfy the executive reporting requirements:

### 1. Core Financial Performance
```dax
Total Revenue = SUM('Sales'[Revenue])
```
```dax
Previous Month Revenue = 
CALCULATE(
    [Total Revenue],
    DATEADD('Calendar'[Date], -1, MONTH)
)
```
```dax
MoM Growth % = 
DIVIDE(
    [Total Revenue] - [Previous Month Revenue],
    [Previous Month Revenue],
    0
)
```

### 2. Dynamic Performance Insight (SWITCH TRUE)
```dax
Performance Insight = 
SWITCH(
    TRUE(),
    [MoM Growth %] >= 0.10, "Revenue is growing strongly.",
    [MoM Growth %] > 0, "Revenue is showing positive growth.",
    [MoM Growth %] = 0, "Revenue is unchanged.",
    [MoM Growth %] > -0.10, "Revenue has declined slightly.",
    [MoM Growth %] <= -0.10, "Revenue has declined significantly.",
    "No historical data available"
)
```

### 3. Dynamic Top Elements
```dax
Top State = 
CALCULATE(
    SELECTEDVALUE('Customers'[State], "Multiple States"),
    TOPN(1, ALL('Customers'[State]), [Total Revenue], DESC)
)
```
```dax
Top Category = 
CALCULATE(
    SELECTEDVALUE('Products'[Category], "Multiple Categories"),
    TOPN(1, ALL('Products'[Category]), [Total Revenue], DESC)
)
```

### 4. Comprehensive Executive Insight (The Narrative)
```dax
Executive Insight = 
"Revenue stands at " & FORMAT([Total Revenue], "₩#,##0.0M") & ". " &
[Performance Insight] & " " &
[Top State] & " is the leading state, while " &
[Top Category] & " is the leading category."
```

### 5. Bonus Challenge: Management Alert
```dax
Management Alert = 
SWITCH(
    TRUE(),
    [MoM Growth %] >= 0.10, "🟢 Positive: Revenue is growing strongly.",
    [MoM Growth %] <= -0.10, "🔴 Attention: Revenue has declined significantly.",
    "🟡 Neutral: Performance stable or tracking closely to previous month."
)
```

---

##  Dashboard Architecture & Features

The dashboard layout is designed for interactive storytelling using three primary layers:
*   **KPI Cards:** Total Revenue, PM Revenue, MoM Growth %, Total Orders, Unique Customers, Average Order Value (AOV).
*   **Visualizations:** Monthly Revenue Trends, Revenue by State, Revenue by Category, and a Top 10 Products by Revenue bar chart.
*   **Context Control:** Global slicers filtering data by **Month**, **State**, and **Category**.
*   **Executive Narrative Block:** A dedicated text box bound to the `[Executive Insight]` measure that changes automatically dynamically as filters adjust.

---



 
