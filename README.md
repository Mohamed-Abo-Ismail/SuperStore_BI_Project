# Super Store Sales Performance Analysis

An end-to-end Business Intelligence solution built with Power BI, covering data cleaning, star schema modeling, DAX measures, and a four-page interactive dashboard with custom Figma-designed dark-theme backgrounds.

Course: DS302 - Data Science Methodology
University: Pharos University, Faculty of Computer Science and Artificial Intelligence
Term: Spring 2025-2026

---

## Overview

This project delivers a complete BI workflow for a fictional US retail company, Superstore, from raw CSV ingestion to a published, interactive dashboard.

**Objectives**

- Build a fully functional, interactive Power BI dashboard for a retail sales organization
- Clean and transform data with Power Query
- Implement a star schema with proper Many-to-One relationships
- Create 13 DAX measures covering revenue, profit, growth, and time intelligence
- Design four analytical report pages plus a home screen with Figma-designed backgrounds

## Business Problem

Management lacks a centralized, visual way to monitor sales performance, customer behavior, product profitability, and regional trends across three product categories: Technology, Furniture, and Office Supplies. This dashboard addresses those needs through interactive, filterable reports.

## Dataset

| Property | Details |
|---|---|
| Source | Kaggle Superstore Sales Dataset (by Vivek Chowdhury) |
| File | Sample - Superstore.csv |
| Rows | 9,994 transactions |
| Columns | 21 (plus an added Profit Status column) |
| Date range | 2014 - 2017 |
| Geography | United States: 4 regions, 49 states |
| Categories | Technology, Furniture, Office Supplies |

## Tech Stack

| Area | Tool |
|---|---|
| Data transformation | Power Query (M) |
| Data modeling | Power BI, star schema |
| Calculations | DAX |
| Visualization | Power BI |
| UI / background design | Figma |
| Publishing | Power BI Service |

## Data Cleaning (Power Query)

Six transformation steps were applied before modeling:

| # | Step | Purpose |
|---|---|---|
| 1 | Remove duplicates (key: Order ID) | Prevent double-counting |
| 2 | Replace nulls in Revenue and Profit with 0 | Avoid errors in DAX aggregations |
| 3 | Enforce data types | Dates as Date, financials as Decimal, Quantity as Whole Number, IDs as Text |
| 4 | Add Profit Status column | Classify each row as Profitable, Break-Even, or Loss |
| 5 | Filter test rows | Remove rows where Order ID is null |
| 6 | Rename columns | Sales to Revenue, Cust ID to CustomerID |

## Data Model (Star Schema)

One fact table connected to four dimension tables through Many-to-One relationships.

```
                 DimProduct              DimRegion
                      \                    /
                       \                  /
                        +---- Fact ------+
                       /                  \
                      /                    \
                DimCustomer              DimDate
```

| Table | Type | Key Fields |
|---|---|---|
| Fact | Fact | OrderID, CustomerID, ProductID, OrderDate, Region, Revenue, Profit, Quantity, Discount, ProfitStatus |
| DimCustomer | Dimension | CustomerID, CustomerName, Segment |
| DimProduct | Dimension | ProductID, ProductName, Category, SubCategory |
| DimDate | Dimension | OrderDate, Year, Month, MonthName, Quarter |
| DimRegion | Dimension | Region |

## DAX Measures

**Basic aggregations**

| Measure | Formula |
|---|---|
| Total Revenue | `SUM(Fact[Revenue])` |
| Total Profit | `SUM(Fact[Profit])` |
| Total Quantity | `SUM(Fact[Quantity])` |
| Total Orders | `DISTINCTCOUNT(Fact[OrderID])` |
| Total Discount | `SUM(Fact[Discount])` |

**Customer and financial intelligence**

| Measure | Formula |
|---|---|
| Average Order Value | `DIVIDE([Total Revenue], [Total Orders])` |
| Profit Margin % | `DIVIDE([Total Profit], [Total Revenue]) * 100` |
| Top Customer Revenue | `MAXX(VALUES(DimCustomer[Customer Name]), [Total Revenue])` |
| Bottom Customer Revenue | `MINX(VALUES(DimCustomer[Customer Name]), [Total Revenue])` |

**Time intelligence**

| Measure | Formula |
|---|---|
| YTD Revenue | `TOTALYTD([Total Revenue], DimDate[OrderDate])` |
| YTD Profit | `TOTALYTD([Total Profit], DimDate[OrderDate])` |
| Previous Year Revenue | `CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(DimDate[OrderDate]))` |
| Revenue Growth % | `DIVIDE([Total Revenue] - [Previous Year Revenue], [Previous Year Revenue]) * 100` |

## Dashboard Pages

The report contains a Home Screen and four analytical pages, each with left-panel slicers for interactivity.

| Page | Theme | Focus | Key Visuals |
|---|---|---|---|
| 1. Revenue Overview | Blue | Overall sales performance | KPI cards, revenue over years, revenue by category, monthly YTD revenue, revenue by segment |
| 2. Customer Analysis | Purple | Customer behavior and segments | Top/bottom customer revenue, Top 10 customers, orders by segment, orders by profit status |
| 3. Product Performance | Red | Profitability by category and sub-category | Profit by sub-category, profit vs. discount scatter, revenue vs. profit by category |
| 4. Regional Analysis | Orange | Geographic and time-based trends | Filled map by state, profit/revenue by region, YTD revenue by region |

## Design Process

The dashboard was built in two stages:

1. Functional layer: a light-themed version was built directly in Power BI to validate visuals, fields, and DAX measures.
2. Visual layer: once the data was verified, dark-themed page backgrounds were designed in Figma and imported as images behind the visuals.

## Key Findings

**Revenue**

- Total revenue for 2014-2017 reached about $1.10M, with 48.11% year-over-year growth in the final year
- The Consumer segment generates the largest share of revenue (50.76%), followed by Corporate (31.15%) and Home Office (18.09%)

**Profitability**

- Total profit is $132.52K, a profit margin of 12.05%
- 81.37% of orders are profitable, 18.07% result in a loss, and 0.56% break even
- Sub-categories such as Tables and Bookcases show negative profit
- Higher discount rates are associated with lower or negative profit

**Regions**

- East generates the highest total profit, followed by West
- Central shows the lowest profit margin
- YTD revenue grows consistently across all four regions

**Customers**

- Top customer: Adrian Barton (about $12.12K in revenue)
- Average order value: $219.58

## Project Deliverables

- Power BI report (.pbix) with a home screen and four analytical pages
- Report published on Power BI Service
- Figma design files for the dashboard backgrounds
- Full project documentation report (PDF)

## Repository Structure

```
.
├── data/
│   └── Sample - Superstore.csv
├── dashboard/
│   └── SuperStore.pbix
├── design/
│   └── figma-backgrounds/
├── docs/
│   └── SuperStore_BI_Report.pdf
└── README.md
```

## How to Run

1. Clone or download this repository.
2. Open dashboard/SuperStore.pbix in Power BI Desktop.
3. If prompted, update the data source path: Transform data > Data source settings, and point it to data/Sample - Superstore.csv.
4. Click Refresh, then use the slicers on each page to explore the data.

## Team

| Name | Role |
|---|---|
| Mohamed El-Sayed Aly | Project team member |
| Ahmed Mahmoud | Project team member |

Supervised by: Dr. Soha and Eng. Hamdy Saied

---

Built for DS302 Data Science Methodology, Pharos University, Spring 2025-2026.
