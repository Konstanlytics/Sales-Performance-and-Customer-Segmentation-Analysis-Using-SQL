# 📌 Project Title: Sales Performance and Customer Segmentation Analysis Using SQL

## 🧩 Project Overview
This SQL project explores a structured approach to understanding sales performance, customer behavior, and product segmentation from a transactional dataset. The goal was to derive meaningful insights that help the business monitor growth trends, measure performance against targets, and identify high-value customers.

---

## 🛠 Tools Used
- **SQL Server** (T-SQL)
- **Azure Data Lake** (assumed storage layer from `gold` schema)
- **Data Source Tables:** `fact_sales`, `dim_products`, `dim_customers`

---

## 📊 Key Analyses & Insights

### 1. 📈 Time-Based Sales Trends
- Aggregated sales metrics by day, month, and year.
- Derived total sales, total quantity sold, and unique customer counts over time.

### 2. 📉 Cumulative Metrics
- Calculated monthly and yearly running totals using window functions to visualize business growth.
- Added moving averages for price trends.

### 3. 🎯 Performance Benchmarking
- Compared product sales performance year-over-year.
- Labeled each product's yearly performance as *Above Avg*, *Below Avg*, or *Avg* based on historical trends.
- Conducted year-over-year comparisons for growth tracking.

### 4. 🧮 Proportional Contribution Analysis
- Identified top-performing product categories by comparing each category’s share of total sales.

### 5. 🔍 Customer Segmentation
- Grouped customers into **VIP**, **Regular**, and **New** based on their lifespan and total spending.
- Created detailed customer reports with KPIs like:
  - Recency
  - Lifespan
  - Average Order Value (AOV)
  - Average Monthly Spend

### 6. 📦 Product Segmentation
- Classified products into cost-based segments (e.g., Below 100, 100–500, 500–1000, Above 1000).
- Counted how many products fall into each segment to support pricing and inventory strategy.

---

## 📌 Business Impact
- Equipped business stakeholders with visibility into sales performance and product demand.
- Helped in targeting high-value customers through intelligent segmentation.
- Enabled effective performance tracking and future planning with moving metrics and benchmarks.
