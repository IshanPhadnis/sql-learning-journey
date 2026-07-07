Question: How can you combine the separate Date and Time columns into a single “Datetime” column?
```sql
SELECT order_date,
order_time,
PARSE_DATETIME("%Y-%m-%d %H:%M:%S",CONCAT(order_date, " ",order_time)) date_time
FROM `flipkart.orders`;
SELECT PARSE_DATETIME("%Y-%m-%d %H:%M:%S", "2024-07-22 07:27:17"),
PARSE_DATETIME("%Y-%m-%d %I:%M:%S %P", "2024-07-22 07:27:17 PM");
```

Question: How many orders are placed annually, and what trends can be observed over the years?
```sql
SELECT EXTRACT(YEAR FROM order_date) year,
COUNT(order_id) total_orders
FROM `flipkart.orders`
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year DESC;
```

Question: Which year had the highest number of high-value orders (orders above ₹10,000)?
```sql
SELECT EXTRACT(YEAR FROM order_date) year,
COUNT(order_id) high_value_orders
FROM `flipkart.orders`
WHERE amount > 10000
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year DESC;
```

Question: What are the maximum and average order amounts recorded each year on Flipkart?
```sql
SELECT EXTRACT(YEAR FROM order_date) year,
MAX(amount) AS max_order_amount,
ROUND(AVG(amount),2) AS avg_order_amount
FROM `flipkart.orders`
GROUP BY EXTRACT(YEAR FROM order_date)
ORDER BY year DESC;
```

Question: How does the number of Flipkart orders vary across different months of each year?
```sql
SELECT FORMAT_DATE("%Y-%m", order_date) year_month,
COUNT(order_id) total_orders
FROM `flipkart.orders`
GROUP BY FORMAT_DATE("%Y-%m", order_date)
ORDER BY year_month DESC;
```

Question: What is the quarterly pattern of Flipkart orders across customer states, and do we observe significant seasonal trends?
```sql
SELECT EXTRACT(YEAR FROM o.order_date) year,
EXTRACT(QUARTER FROM o.order_date) quarter,
c.state,
COUNT(o.order_id) total_orders,
SUM(o.amount) total_revenue,
ROUND(AVG(o.amount),2) avg_order_value
FROM `flipkart.orders` o JOIN `flipkart.customers` c
ON o.customer_id = c.customer_id
GROUP BY EXTRACT(YEAR FROM o.order_date),
EXTRACT(QUARTER FROM o.order_date),
c.state
ORDER BY year DESC, quarter DESC, total_orders;
```


Question: How many Flipkart orders have been placed so far in the current month for the year 2026?
```sql
SELECT COUNT(order_id) AS orders_this_month
FROM `flipkart.orders`
WHERE EXTRACT(YEAR FROM order_date) = 2026
AND EXTRACT(MONTH FROM order_date) = EXTRACT(MONTH FROM CURRENT_DATE());
```<img width="1024" height="1536" alt="download" src="https://github.com/user-attachments/assets/f043dc66-1d17-4513-b8f8-5c0827fb3dd7" />


Question: In the past 90 days (from the last recorded date, September 30, 2025), how many Flipkart orders were placed, and what
were the average and maximum order amounts?
```sql
SELECT COUNT(order_id) total_orders,
MAX(amount) max_order_amt,
AVG(amount) avg_order_amt
FROM `flipkart.orders`
WHERE order_date >= DATE_SUB("2025-09-30", INTERVAL 90 DAY);
SELECT DATE_SUB("2025-09-30", INTERVAL 90 DAY); -- 2025-07-02
```
Question: Calculate the number of orders placed in the first 45 days of 2025.
```sql
SELECT
COUNT(*) AS total_orders_first_45_days
FROM `flipkart.orders`
WHERE order_date BETWEEN DATE '2025-01-01' AND DATE_ADD(DATE '2025-01-01', INTERVAL 45 DAY);
```
