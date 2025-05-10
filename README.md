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

### 📊 SQL Query:
SELECT 
    order_date,
    SUM(sales_amount) AS Total_Sales,
    COUNT(DISTINCT customer_key) AS Total_Customers,
    SUM(quantity) AS Total_Quantity
FROM [gold.fact_sales]
WHERE order_date IS NOT NULL
GROUP BY order_date
ORDER BY order_date;

## 2. 📉 Cumulative Metrics

Calculated monthly and yearly running totals using window functions to visualize business growth.

Added moving averages for price trends.

### 📊 SQL Query:

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

## 3. 🎯 Performance Benchmarking

Compared product sales performance year-over-year.

- Labeled each product's yearly performance as **Above Avg**, **Below Avg**, or **Avg** based on historical trends.
- Conducted year-over-year comparisons for growth tracking.

### 📊 SQL Query:

WITH yearly_product_sales AS (
    SELECT 
        YEAR(f.order_date) AS order_year,
        p.product_name,
        SUM(f.sales_amount) AS current_sales
    FROM [gold.fact_sales] f
    LEFT JOIN [gold.dim_products] p ON f.product_key = p.product_key
    WHERE f.order_date IS NOT NULL
    GROUP BY YEAR(f.order_date), p.product_name
)
SELECT 
    order_year,
    product_name,
    current_sales,
    AVG(current_sales) OVER (PARTITION BY product_name) AS avg_sales
FROM yearly_product_sales
ORDER BY product_name, order_year;

## 4. 🧮 Proportional Contribution Analysis

Identified top-performing product categories by comparing each category’s share of total sales.

### 📊 SQL Query:

WITH category_sales AS (
    SELECT
        category,
        SUM(sales_amount) AS total_sales
    FROM [gold.fact_sales] f
    LEFT JOIN [gold.dim_products] p ON p.product_key = f.product_key
    GROUP BY category
)
SELECT
    category,
    total_sales,
    CONCAT(ROUND((CAST(total_sales AS FLOAT) / SUM(total_sales) OVER ()) * 100, 2), '%') AS Percentage_of_Total
FROM category_sales
ORDER BY total_sales DESC;

## 5. 🔍 Customer Segmentation

Segmented customers into VIP, Regular, and New tiers and generated behavioral KPIs:

- **Recency** – months since last purchase  
- **Lifespan** – active months on record  
- **Average Order Value (AOV)**  
- **Average Monthly Spend**

### 📊 SQL Query:

```sql
WITH customer_spending AS (
    SELECT
        c.customer_key,
        SUM(f.sales_amount)                         AS total_spending,
        MIN(order_date)                             AS first_order,
        MAX(order_date)                             AS last_order,
        DATEDIFF(MONTH, MIN(order_date), MAX(order_date)) AS lifespan
    FROM [gold.fact_sales] f
    LEFT JOIN [gold.dim_customers] c
           ON f.customer_key = c.customer_key
    GROUP BY c.customer_key
)
SELECT
    customer_segment,
    COUNT(customer_key) AS total_customers
FROM (
    SELECT
        customer_key,
        CASE
            WHEN lifespan >= 12 AND total_spending > 5000 THEN 'VIP'
            WHEN lifespan >= 12 AND total_spending <= 5000 THEN 'Regular'
            ELSE 'New'
        END AS customer_segment
    FROM customer_spending
) t
GROUP BY customer_segment
ORDER BY total_customers DESC;


```sql
WITH customer_spending AS (
    SELECT
        c.customer_key,
        SUM(f.sales_amount)                         AS total_spending,
        MIN(order_date)                             AS first_order,
        MAX(order_date)                             AS last_order,
        DATEDIFF(MONTH, MIN(order_date), MAX(order_date)) AS lifespan
    FROM [gold.fact_sales] f
    LEFT JOIN [gold.dim_customers] c
           ON f.customer_key = c.customer_key
    GROUP BY c.customer_key
)
SELECT
    customer_segment,
    COUNT(customer_key) AS total_customers
FROM (
    SELECT
        customer_key,
        CASE
            WHEN lifespan >= 12 AND total_spending > 5000 THEN 'VIP'
            WHEN lifespan >= 12 AND total_spending <= 5000 THEN 'Regular'
            ELSE 'New'
        END AS customer_segment
    FROM customer_spending
) t
GROUP BY customer_segment
ORDER BY total_customers DESC;

## 6. 📦 Product Segmentation

Classified products into cost-based buckets to aid pricing and inventory strategy:

- **Below 100**
- **100 – 500**
- **500 – 1000**
- **Above 1000**

### 📊 SQL Query:

WITH product_segments AS (
    SELECT
        product_key,
        product_name,
        cost,
        CASE
            WHEN cost < 100 THEN 'Below 100'
            WHEN cost BETWEEN 100 AND 500 THEN '100-500'
            WHEN cost BETWEEN 500 AND 1000 THEN '500-1000'
            ELSE 'Above 1000'
        END AS cost_range
    FROM [gold.dim_products]
)
SELECT
    cost_range,
    COUNT(product_key) AS total_products
FROM product_segments
GROUP BY cost_range
ORDER BY total_products DESC;


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




