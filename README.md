# Vendor Intelligence Dashboard
### Built with Microsoft Power BI | The Build Fellowship by Open Avenues

---

## Project Overview

This project involves building two fully interactive Power BI dashboards 
to analyze vendor revenue and margin performance across 16 vendors 
spanning Q1 2019 to Q2 2025. The data covers $52M+ in total revenue 
and provides actionable business insights on profitability and goal attainment.

This project was completed as part of the **Build Fellowship by Open Avenues** 
experiential learning program.

---

## Dashboard Screenshots

### Dashboard 1 — Vendor Intelligence Dashboard
![Vendor Intelligence Dashboard](dashboard1_vendor_intelligence.png)

### Dashboard 2 — Vendor Margin Intelligence Dashboard
![Vendor Margin Intelligence Dashboard](dashboard2_vendor_margin.png)

---

## The Business Problem

The vendor portfolio was generating strong revenue but there was no clear 
visibility into which vendors were actually profitable. The goal was to 
build an interactive reporting solution that could answer:

- Which vendors are generating the most revenue?
- Which vendors are meeting the 20% margin goal?
- How has revenue and margin trended over time?
- Where are the biggest opportunities for improvement?

---

## Key Insights Uncovered

- **Only 2 out of 16 vendors** consistently meet the 20% margin goal
- **Spot AI** is the top revenue vendor at $17.4M but operates at only 15.4% margin — the biggest margin improvement opportunity in the portfolio
- **Strongarm** has the best margin at 27.7% and is the benchmark vendor for profitability
- **Cisco Q4 2024** was the biggest anomaly — $2.4M in invoices but only 6.1% margin
- Portfolio average margin of **13.8%** is significantly below the 20% target

---

## What I Built

### Dashboard 1 — Vendor Intelligence Dashboard
Covers overall revenue performance from Q1 2019 to Q2 2025

| Visual | Description |
|---|---|
| KPI Cards | Total Revenue, Top Vendor, Avg Margin, Goal Attainment |
| Line Chart | Quarterly Revenue Trend across 26 quarters |
| Donut Chart | Revenue Share by Vendor |
| Bar Chart | Revenue by Vendor ranked highest to lowest |
| Bar Chart | Avg Margin vs 20% Goal with conditional formatting |
| Table | Vendor Scorecard with revenue, margin, and goal status |
| Slicers | Year filter and Vendor dropdown |

### Dashboard 2 — Vendor Margin Intelligence Dashboard
Covers detailed invoice and margin analysis from Q1 2021 to Q2 2025

| Visual | Description |
|---|---|
| KPI Cards | Total Invoice, Total Margin, Avg Margin %, Best Vendor, Worst Vendor |
| Line Chart | Margin % Trend by Quarter |
| Bar Chart | Margin % by Vendor vs 20% Goal |
| Grouped Bar | Invoice vs Margin $ by Vendor |
| Stacked Bar | Margin $ by Year per Vendor |
| Table | Detailed Margin Scorecard with best quarter per vendor |
| Slicers | Year, Vendor, and Quarter filters |

---

## Tools and Technologies Used

| Tool | Purpose |
|---|---|
| Microsoft Power BI Desktop | Dashboard development and visualization |
| Power Query | Data cleaning and transformation |
| DAX (Data Analysis Expressions) | Custom measures and KPI calculations |
| Microsoft Excel | Source data |
| Star Schema Modeling | Data model design |

---

## Data Preparation Steps

1. Loaded raw Excel file containing two separate data tables into Power BI
2. Cleaned data in Power Query — removed duplicates, blank rows, and unnamed columns
3. Unpivoted wide quarterly columns into rows for time series analysis
4. Created a clean Quarter column using custom formulas to standardize formats
5. Added QuarterSort column to enable chronological sorting
6. Built a VendorList lookup table for proper Star Schema relationships
7. Established Many-to-One relationships between fact and dimension tables

---

## DAX Measures Created

```dax
-- Total Revenue
Total Revenue = SUM(VendorRevenue[Revenue])

-- Average Margin Percentage
Avg Margin Pct = 
FORMAT(
    DIVIDE(
        SUM(VendorMargins_Detail[Margin]),
        SUM(VendorMargins_Detail[Invoice])
    ),
"0.0%")

-- Goal Attainment
Goal Attainment = 
VAR AboveGoal =
    COUNTROWS(
        FILTER(
            VendorList,
            CALCULATE(
                DIVIDE(
                    SUM(VendorMargins_Detail[Margin]),
                    SUM(VendorMargins_Detail[Invoice])
                )
            ) >= 0.20
        )
    )
VAR TotalVendors = COUNTROWS(VendorList)
RETURN
    FORMAT(AboveGoal,"0") & " / " & FORMAT(TotalVendors,"0")

-- Best Margin Vendor
Best Margin Vendor = 
MAXX(
    TOPN(
        1,
        FILTER(
            VendorList,
            CALCULATE(
                DIVIDE(
                    SUM(VendorMargins_Detail[Margin]),
                    SUM(VendorMargins_Detail[Invoice])
                )
            ) > 0
        ),
        CALCULATE(
            DIVIDE(
                SUM(VendorMargins_Detail[Margin]),
                SUM(VendorMargins_Detail[Invoice])
            )
        ),
        DESC
    ),
    VendorList[Vendor]
)
```

---

## Key Business Recommendations

1. **Renegotiate Spot AI pricing** — largest revenue vendor but below margin goal. Even a 5% improvement adds ~$870K in profit
2. **Protect Strongarm and Fortinet** — only vendors consistently above 20% goal. Prioritize long term contracts
3. **Investigate Cisco Q4 2024** — $2.4M invoice at only 6.1% margin is the worst single quarter in the dataset
4. **Review Zscaler, Forcepoint, and Wiz** — minimal revenue and margins below 5%. Evaluate whether these relationships are worth maintaining
5. **Set quarterly margin reviews** — portfolio average of 13.8% needs a structured improvement plan to reach the 20% goal

---

## Project Context

This project was completed as part of the **Build Fellowship by Open Avenues** 
experiential learning program. It is a simulation of real-world business analytics 
work designed to develop practical data skills.

**Role:** Build Student Consultant, Data Analytics
**Program:** The Build Fellowship by Open Avenues
**Duration:** 2025

---

## Connect With Me

Feel free to connect with me on LinkedIn if you want to discuss this project 
or data analytics in general!

(https://www.linkedin.com/in/shruthi-gs/)
