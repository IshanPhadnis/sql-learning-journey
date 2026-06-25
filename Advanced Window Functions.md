# Flipkart SQL Case Study

## 📌 Problem Statement

Flipkart, one of India's leading e-commerce platforms, wants to strengthen its competitive position by improving customer retention, optimizing logistics, and increasing sales efficiency.

The dataset contains information about:
- Customers
- Products
- Orders
- Order Items

**Time Period:** January 2023 – September 2025

---

## Dataset

[Google Drive Dataset Link](https://drive.google.com/drive/folders/1IJ8rjUVSS98T5W5npt611N0pk-RZs6NK)

---

## Question 1

**Display the average order amount of all customers in a new column for each order in the orders table.**

```sql
SELECT
    order_id,
    customer_id,
    amount,
    order_date,
    AVG(amount) OVER () AS avg_order_amount
FROM `flipkart.orders`;
```

---

## Question 2

**For each order in the orders table, display the order amount along with the total amount spent by the customer across all their orders in a new column.**

```sql
SELECT
    order_id,
    customer_id,
    amount,
    order_date,
    SUM(amount) OVER (PARTITION BY customer_id) AS total_cust_spent
FROM `flipkart.orders`;
```
