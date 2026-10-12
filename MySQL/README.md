# MySQL Analytics Scripts

This folder contains the database queries used to extract foundational monthly metrics and evaluate month-on-month growth trends for the coffee sales data.

## Base Metrics & Trend Queries

### 1. Total Revenue / Sales Analysis

sql
-- Calculate Total Sales per month
SELECT 
    MONTH(transaction_date) AS month_num,
    ROUND(SUM(unit_price * transaction_qty)) AS total_sales
FROM 
    coffee_sales
GROUP BY 
    MONTH(transaction_date)
ORDER BY 
    month_num;

-- Month-on-Month Sales Difference and Growth Rate
SELECT 
    month_num,
    total_sales,
    total_sales - LAG(total_sales) OVER (ORDER BY month_num) AS sales_difference,
    ROUND(
        (
            (total_sales - LAG(total_sales) OVER (ORDER BY month_num)) 
            / LAG(total_sales) OVER (ORDER BY month_num)
        ) * 100, 
        2
    ) AS mom_growth_pct
FROM 
    (
        SELECT 
            MONTH(transaction_date) AS month_num,
            SUM(unit_price * transaction_qty) AS total_sales
        FROM 
            coffee_sales
        GROUP BY 
            MONTH(transaction_date)
    ) subquery;


### 2. Total Transaction Orders Analysis

sql
-- Calculate Total Orders per month
SELECT 
    MONTH(transaction_date) AS month_num,
    COUNT(DISTINCT transaction_id) AS total_orders
FROM 
    coffee_sales
GROUP BY 
    MONTH(transaction_date)
ORDER BY 
    month_num;

-- Month-on-Month Order Ticket Trends
SELECT 
    month_num,
    total_orders,
    total_orders - LAG(total_orders) OVER (ORDER BY month_num) AS order_difference,
    ROUND(
        (
            (total_orders - LAG(total_orders) OVER (ORDER BY month_num)) 
            / LAG(total_orders) OVER (ORDER BY month_num)
        ) * 100, 
        2
    ) AS mom_order_growth_pct
FROM 
    (
        SELECT 
            MONTH(transaction_date) AS month_num,
            COUNT(DISTINCT transaction_id) AS total_orders
        FROM 
            coffee_sales
        GROUP BY 
            MONTH(transaction_date)
    ) subquery;



