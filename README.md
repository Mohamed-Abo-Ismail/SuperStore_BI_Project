# Super Store Sales Performance Analysis

An end-to-end Power BI project for a fictional US retail company: data cleaning, star schema modeling, DAX measures, and a four-page interactive dashboard with Figma-designed backgrounds.

---

## Overview

Management lacks a centralized, visual way to monitor sales performance, customer behavior, product profitability, and regional trends. This project delivers a Power BI dashboard that covers those needs, from raw CSV to published report.

## Key Findings

- Total revenue is about $1.10M, with 48.11% year-over-year growth in the final year
- Total profit is $132.52K, a 12.05% profit margin
- Consumer is the largest segment at 50.76% of revenue
- 81.37% of orders are profitable, 18.07% lose money, and 0.56% break even
- Tables and Bookcases are loss-making sub-categories, and higher discounts are linked to lower profit
- East has the highest profit and Central the lowest margin
- Top customer: Adrian Barton (about $12.12K); average order value: $219.58

## Dataset

Kaggle Superstore Sales Dataset (Sample - Superstore.csv): 9,994 transactions, 21 columns, 2014-2017, across 4 US regions and 49 states. Categories: Technology, Furniture, Office Supplies.

## Tools

Power BI, Power Query, DAX, Figma, Power BI Service

## Data Cleaning (Power Query)

1. Removed duplicates using Order ID
2. Replaced nulls in Revenue and Profit with 0
3. Enforced correct data types
4. Added a Profit Status column (Profitable, Break-Even, Loss)
5. Removed rows with a null Order ID
6. Renamed columns (Sales to Revenue, Cust ID to CustomerID)

## Data Model

Star schema with one fact table (Fact) and four dimension tables (DimCustomer, DimProduct, DimDate, DimRegion), connected by Many-to-One relationships.

## DAX Measures

| Measure | Formula |
|---|---|
| Total Revenue | `SUM(Fact[Revenue])` |
| Total Profit | `SUM(Fact[Profit])` |
| Total Quantity | `SUM(Fact[Quantity])` |
| Total Orders | `DISTINCTCOUNT(Fact[OrderID])` |
| Total Discount | `SUM(Fact[Discount])` |
| Average Order Value | `DIVIDE([Total Revenue], [Total Orders])` |
| Profit Margin % | `DIVIDE([Total Profit], [Total Revenue]) * 100` |
| Top Customer Revenue | `MAXX(VALUES(DimCustomer[Customer Name]), [Total Revenue])` |
| Bottom Customer Revenue | `MINX(VALUES(DimCustomer[Customer Name]), [Total Revenue])` |
| YTD Revenue | `TOTALYTD([Total Revenue], DimDate[OrderDate])` |
| YTD Profit | `TOTALYTD([Total Profit], DimDate[OrderDate])` |
| Previous Year Revenue | `CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(DimDate[OrderDate]))` |
| Revenue Growth % | `DIVIDE([Total Revenue] - [Previous Year Revenue], [Previous Year Revenue]) * 100` |

## Dashboard Pages

| Page | Focus |
|---|---|
| Home Screen | Title, team, and navigation button |
| Revenue Overview | Revenue by year, category, and segment; monthly YTD revenue |
| Customer Analysis | Top 10 customers, segments, orders by profit status |
| Product Performance | Profit by sub-category, profit vs. discount, revenue vs. profit |
| Regional Analysis | Revenue by state, regional profit and revenue, YTD trends |

Each page has slicers for Year, Region, and Category (or Segment).
