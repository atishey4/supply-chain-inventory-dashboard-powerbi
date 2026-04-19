# Supply Chain Efficiency & Inventory Optimization Dashboard

## Objective
Build an interactive Power BI dashboard to monitor supply chain KPIs (inventory turnover, days of inventory on hand, stockouts, fill rate, on-time delivery, lead time) and compute optimization policies (EOQ, safety stock, reorder point) per SKU.

---

## Tools Used
- **Power BI Desktop** – Dashboard design, data modeling, and report building
- **Power Query Editor** – Data cleaning and transformation
- **DAX (Data Analysis Expressions)** – KPI and measure creation
- **Microsoft Excel / CSV** – Source dataset
- **Python (optional)** – Precomputing EOQ, Safety Stock, and Reorder Point per SKU

---

## Dataset Description

A realistic supply chain dataset containing **500+ rows** with the following columns:

| Column | Description |
|---|---|
| Date | Transaction / snapshot date |
| Month | Month name extracted from date |
| Quarter | Q1 / Q2 / Q3 / Q4 |
| Year | Fiscal year |
| Product ID | Unique product identifier |
| Product Name | Name of the product / SKU |
| Product Category | Raw Material, Safety Equipment, Machinery, Electrical, Packaging, Lubricants |
| Supplier Name | Name of the supplying vendor |
| Warehouse Location | Physical warehouse or distribution center |
| Region | East, North, South, West |
| Opening Stock | Stock level at the start of the period |
| Incoming Stock | Stock received during the period |
| Units Sold | Units dispatched / sold during the period |
| Closing Stock | Stock remaining at end of period |
| Reorder Level | Minimum stock threshold before reordering |
| Reorder Status | Whether a reorder has been triggered |
| Lead Time (Days) | Days between placing and receiving an order |
| Order Quantity | Quantity ordered per purchase order |
| Delivery Status | On Time / Delayed |
| Delivery Time (Days) | Actual days taken for delivery |
| Inventory Holding Cost | Cost of holding inventory for the period |
| Stockout Status | Yes / No – whether stock ran out |
| Demand Forecast | Forecasted demand for the period |
| Actual Demand | Actual demand recorded |
| Forecast Accuracy % | Accuracy of demand forecast vs actual |
| Inventory Turnover Ratio | COGS / Average Inventory for the period |

---

## Power Query Steps

1. **Remove null and blank values** – Cleaned rows with missing Product ID, Closing Stock, or Delivery Status to ensure accurate inventory and delivery calculations
2. **Change data types** – Set Inventory Holding Cost as Currency; Units Sold, Opening Stock, Closing Stock as Whole Number; Date as Date type
3. **Create Month, Quarter, Year columns** – Extracted from the Date column for time-based trend filtering and analysis
4. **Handle duplicates** – Removed duplicate Product ID + Date combinations to maintain one record per product per period
5. **Rename columns** – Standardized column names for clarity (e.g., "Lead_Time" → "Lead Time (Days)")
6. **Format number, currency, and percentage fields** – Applied proper formatting for Inventory Holding Cost, Forecast Accuracy %, and Turnover Ratio
7. **Create calculated columns** – Added Stockout Status flag and Reorder Status indicator directly in Power Query
8. **Standardize supplier names and product categories** – Trimmed whitespace and applied consistent casing to Supplier Name and Product Category fields

---

## DAX Measures

```dax
Total Closing Stock = SUM(SupplyChainData[Closing Stock])

Total Opening Stock = SUM(SupplyChainData[Opening Stock])

Total Incoming Stock = SUM(SupplyChainData[Incoming Stock])

Total Units Sold = SUM(SupplyChainData[Units Sold])

Stockout Count = CALCULATE(COUNTROWS(SupplyChainData), SupplyChainData[Stockout Status] = "Yes")

Reorder Count = CALCULATE(COUNTROWS(SupplyChainData), SupplyChainData[Reorder Status] = "Yes")

Avg Lead Time = AVERAGE(SupplyChainData[Lead Time (Days)])

On-Time Delivery % =
    DIVIDE(
        CALCULATE(COUNTROWS(SupplyChainData), SupplyChainData[Delivery Status] = "On Time"),
        COUNTROWS(SupplyChainData),
        0
    ) * 100

Avg Forecast Accuracy % = AVERAGE(SupplyChainData[Forecast Accuracy %])

Total Inventory Holding Cost = SUM(SupplyChainData[Inventory Holding Cost])

Avg Inventory Turnover Ratio = AVERAGE(SupplyChainData[Inventory Turnover Ratio])
```

---

## Dashboard Features

- **KPI Cards** – Total Closing Stock, Stockout Count, On-Time Delivery %, Avg Forecast Accuracy %, Avg Inventory Turnover Ratio, Total Inventory Holding Cost displayed at the top
- **Column Chart** – Total Closing Stock by Product Category showing inventory distribution across categories
- **Clustered Bar Chart** – Demand Forecast and Total Units Sold by Product Category for forecast vs actuals comparison
- **Line Chart** – Total Units Sold and Total Closing Stock by Month Name tracking inventory and demand trends over the year
- **Donut Chart** – Total Units Sold by Delivery Status (On Time vs Delayed distribution)
- **Matrix Table** – Product Name, Closing Stock, Reorder Level, Lead Time (Days), Delivery Status, and Stockout Status for item-level operational review
- **Slicers & Filters** – Region, Supplier Name, Product Category

---

## Key Insights

- **Total Closing Stock stands at 38K units** with a Reorder Level total of approximately 23,960, indicating several products are near or below their reorder thresholds
- **Stockout Count of 5** highlights specific SKUs requiring urgent replenishment attention
- **On-Time Delivery % is 83.1%**, meaning approximately 17% of deliveries are delayed, with Delayed status dominating the delivery mix at 88.11% by volume
- **Raw Material** category holds the highest closing stock, followed by Safety Equipment and Machinery
- **Avg Forecast Accuracy of 94.1%** indicates demand planning is reliable, though category-level variance may exist
- **Avg Inventory Turnover Ratio of 5.26** reflects moderately efficient inventory utilization across the portfolio
- Products like **Conveyor Belt, Bearing Set, and Hard Hat** are below reorder level with delayed delivery status, posing the highest stockout risk
- **Total Inventory Holding Cost of ₹1.35M** presents an opportunity for cost reduction through leaner inventory management in slow-moving categories

---

## Screenshots

### Dashboard Overview
![Supply Chain Dashboard](screenshots/dashboard_overview.png)

> *Add your Power BI dashboard screenshot here. Export from Power BI → File → Export → Export to PDF or take a screenshot and save in the `screenshots/` folder.*

---

## Conclusion

This Supply Chain Efficiency & Inventory Optimization Dashboard gives supply chain analysts, operations managers, and inventory planners a comprehensive, interactive view of inventory health and fulfillment performance. By bringing together stock levels, delivery timelines, demand forecasts, reorder triggers, and holding costs into a single Power BI report, it enables organizations to identify stockout risks early, evaluate supplier reliability, reduce excess inventory costs, and align procurement with actual demand. The dashboard is directly applicable for Supply Chain Analysts, Inventory Planners, Procurement Analysts, Operations Managers, and business leadership in manufacturing, retail, FMCG, and e-commerce environments.
