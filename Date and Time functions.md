# 🛒 Flipkart SQL Case Study

## 📌 Problem Statement

Flipkart, one of India's leading e-commerce platforms, aims to improve customer retention, optimize logistics, and enhance sales performance through data-driven decision-making.

This case study focuses on analyzing Flipkart's order and customer data using SQL date and time functions to answer real-world business questions related to sales trends, seasonality, customer purchasing behavior, and order activity over time.

The dataset contains information about:

* 👤 Customers
* 📦 Products
* 🛍️ Orders
* 🧾 Order Items

**📅 Time Period:** January 2023 – September 2025

---

## 📂 Dataset

**Google Drive Dataset:**
https://drive.google.com/drive/folders/1IJ8rjUVSS98T5W5npt611N0pk-RZs6NK

---

# 📅 Date & Time Functions

## Question 1

**How can you combine the separate `order_date` and `order_time` columns into a single `DATETIME` column?**

```sql
SELECT
    order_date,
    order_time,
    PARSE_DATETIME(
        "%Y-%m-%d %H:%M:%S",
        CONCAT(order_date, " ", order_time)
    ) AS date_time
FROM `flipkart.orders`;
```

### Additional Examples

```sql
SELECT
PARSE_DATETIME("%Y-%m-%d %H:%M:%S","2024-07-22 07:27:17"),
PARSE_DATETIME("%Y-%m-%d %I:%M:%S %P","2024-07-22 07:27:17 PM");
```

---

## Question 2

**How many orders are placed annually, and what trends can be observed over the years?**

```sql
SELECT
    EXTRACT(YEAR FROM order_date) AS year,
    COUNT(order_id) AS total_orders
FROM `flipkart.orders`
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year DESC;
```

---

## Question 3

**Which year had the highest number of high-value orders (orders above ₹10,000)?**

```sql
SELECT
    EXTRACT(YEAR FROM order_date) AS year,
    COUNT(order_id) AS high_value_orders
FROM `flipkart.orders`
WHERE amount > 10000
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year DESC;
```

---

## Question 4

**What are the maximum and average order amounts recorded each year on Flipkart?**

```sql
SELECT
    EXTRACT(YEAR FROM order_date) AS year,
    MAX(amount) AS max_order_amount,
    ROUND(AVG(amount),2) AS avg_order_amount
FROM `flipkart.orders`
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year DESC;
```

---

## Question 5

**How does the number of Flipkart orders vary across different months of each year?**

```sql
SELECT
    FORMAT_DATE("%Y-%m", order_date) AS year_month,
    COUNT(order_id) AS total_orders
FROM `flipkart.orders`
GROUP BY FORMAT_DATE("%Y-%m", order_date)
ORDER BY year_month DESC;
```

---

## Question 6

**What is the quarterly pattern of Flipkart orders across customer states, and are there any significant seasonal trends?**

```sql
SELECT
    EXTRACT(YEAR FROM o.order_date) AS year,
    EXTRACT(QUARTER FROM o.order_date) AS quarter,
    c.state,
    COUNT(o.order_id) AS total_orders,
    SUM(o.amount) AS total_revenue,
    ROUND(AVG(o.amount),2) AS avg_order_value
FROM `flipkart.orders` o
JOIN `flipkart.customers` c
ON o.customer_id = c.customer_id
GROUP BY
    EXTRACT(YEAR FROM o.order_date),
    EXTRACT(QUARTER FROM o.order_date),
    c.state
ORDER BY
    year DESC,
    quarter DESC,
    total_orders;
```

---

## Question 7

**How many Flipkart orders have been placed so far in the current month for the year 2026?**

```sql
SELECT
    COUNT(order_id) AS orders_this_month
FROM `flipkart.orders`
WHERE EXTRACT(YEAR FROM order_date) = 2026
AND EXTRACT(MONTH FROM order_date) = EXTRACT(MONTH FROM CURRENT_DATE());
```

---

## Question 8

**In the past 90 days (from the last recorded date, September 30, 2025), how many Flipkart orders were placed, and what were the average and maximum order amounts?**

```sql
SELECT
    COUNT(order_id) AS total_orders,
    MAX(amount) AS max_order_amt,
    AVG(amount) AS avg_order_amt
FROM `flipkart.orders`
WHERE order_date >= DATE_SUB(
    DATE '2025-09-30',
    INTERVAL 90 DAY
);
```

### Verify the Start Date

```sql
SELECT DATE_SUB(
    DATE '2025-09-30',
    INTERVAL 90 DAY
);
```

---

## Question 9

**Calculate the number of orders placed in the first 45 days of 2025.**

```sql
SELECT
    COUNT(*) AS total_orders_first_45_days
FROM `flipkart.orders`
WHERE order_date BETWEEN
DATE '2025-01-01'
AND DATE_ADD(
    DATE '2025-01-01',
    INTERVAL 45 DAY
);
```

---

## Question 10

**For each order, calculate the number of days since the customer's previous order.**

```sql
SELECT
    customer_id,
    order_id,
    order_date,
    LAG(order_date) OVER(
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS prev_order_date,

    DATE_DIFF(
        order_date,
        LAG(order_date) OVER(
            PARTITION BY customer_id
            ORDER BY order_date
        ),
        DAY
    ) AS days_since_prev_order
FROM `flipkart.orders`;
```

---

## Question 11

**How many days have elapsed since the most recent high-value Flipkart order (order amount > ₹10,000), considering September 30, 2025 as the last recorded date?**

```sql
SELECT
    DATE_DIFF(
        DATE '2025-09-30',
        MAX(order_date),
        DAY
    ) AS days_elapsed_since_last_order
FROM `flipkart.orders`
WHERE amount > 10000;
```

### Verification

```sql
SELECT
    MAX(order_date)
FROM `flipkart.orders`;
```

---

# 📚 SQL Concepts Covered

* Date & Time Functions
* `PARSE_DATE()`
* `PARSE_DATETIME()`
* `FORMAT_DATE()`
* `EXTRACT()`
* `DATE_ADD()`
* `DATE_SUB()`
* `DATE_DIFF()`
* `CURRENT_DATE()`
* `LAG()`
* Aggregate Functions (`COUNT`, `SUM`, `AVG`, `MAX`)
* Common Table Expressions (CTEs)
* Joins
* Time-Series Analysis
* Trend Analysis
* Quarterly & Monthly Reporting

---

⭐ **If you found this repository helpful, feel free to star it!**
