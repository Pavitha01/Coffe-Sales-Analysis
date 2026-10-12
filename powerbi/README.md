# Power BI Dashboard & DAX Measures

This folder houses the interactive .pbix dashboard application. To compute dynamic monthly metrics that react instantly to date slicers, custom data modeling and *DAX (Data Analysis Expressions)* measures were implemented.

## Key DAX Measure Definitions

### 1. Total Sales Metrics
dax
// Core Total Revenue
Total Sales = SUMX('coffee_sales', 'coffee_sales'[unit_price] * 'coffee_sales'[transaction_qty])

// Prior Month Revenue baseline
Previous Month Sales = 
CALCULATE(
    [Total Sales], 
    DATEADD('CalendarTable'[Date], -1, MONTH)
)

// Absolute Value Change MoM
Sales Change Value = [Total Sales] - [Previous Month Sales]

// Percentage Growth MoM
Sales MoM Growth % = DIVIDE([Sales Change Value], [Previous Month Sales], 0)


### 2. Total Orders Metrics
dax
// Distinct order volume count
Total Orders = DISTINCTCOUNT('coffee_sales'[transaction_id])

// Prior Month Order Ticket baseline
Previous Month Orders = 
CALCULATE(
    [Total Orders], 
    DATEADD('CalendarTable'[Date], -1, MONTH)
)

// Absolute Change in Order Volume
Orders Change Value = [Total Orders] - [Previous Month Orders]

// Percentage Change in Orders MoM
Orders MoM Growth % = DIVIDE([Orders Change Value], [Previous Month Orders], 0)


### 3. Total Quantity Sold Metrics
dax
// Net physical units shifted
Total Quantity Sold = SUM('coffee_sales'[transaction_qty])

// Prior Month Units baseline
Previous Month Quantity = 
CALCULATE(
    [Total Quantity Sold], 
    DATEADD('CalendarTable'[Date], -1, MONTH)
)

// Net Unit Shift Variance
Quantity Change Value = [Total Quantity Sold] - [Previous Month Quantity]

// Percentage Volume Change MoM
Quantity MoM Growth % = DIVIDE([Quantity Change Value], [Previous Month Quantity], 0)
