# 📊 **SQL SALES & CUSTOMER ANALYSIS PROJECT**

## 🔍 **Project Overview**
This SQL project explores a structured approach to understanding sales performance, customer behavior, and product segmentation from a transactional dataset. The goal was to derive meaningful insights that help the business monitor growth trends, measure performance against targets, and identify high-value customers.

---

## 🛠 **Tools Used**
- **SQL Server** SSMS
- **Data Source Tables:** `fact_sales`, `dim_products`, `dim_customers`

---

## 📊 **Key Analyses & Insights**

### 1. 📈 **Time-Based Sales Trends**
- Aggregated sales metrics by day, month, and year.
- Derived total sales, total quantity sold, and unique customer counts over time.

#### 📆 **SQL Query Examples:**
```sql
SELECT 
    order_date,
    SUM(sales_amount) AS Total_Sales,
    COUNT(DISTINCT customer_key) AS Total_Customers,
    SUM(quantity) AS Total_Quantity
FROM [gold.fact_sales]
WHERE order_date IS NOT NULL
GROUP BY order_date
ORDER BY order_date;

### 2. 📉 **Cumulative Metrics**
- Calculated monthly and yearly running totals using window functions to visualize business growth.

- Added moving averages for price trends

#### 📊 **SQL Query Examples:**
```sql
SELECT 
    order_date,
    total_sales,
    SUM(total_sales) OVER (ORDER BY order_date) AS Running_Total_Sales
FROM (
    SELECT
        DATETRUNC(MONTH, order_date) AS order_date,
        SUM(sales_amount) AS total_sales
    FROM [gold.fact_sales]
    WHERE order_date IS NOT NULL
    GROUP BY DATETRUNC(MONTH, order_date)
) t;
