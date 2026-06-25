# Flipkart SQL Case Study

## Problem Statement

Flipkart, one of India's leading e-commerce platforms, wants to strengthen its competitive position by improving customer retention, optimizing logistics, and increasing sales efficiency. With growing competition in the online retail space, the company's leadership has tasked the analytics team to leverage customer, product, and order data to uncover actionable insights.

The dataset contains information about **customers, products, orders, and order items** from **January 2023 to September 2025**.

---

# Question 1 : Display customers who have placed orders with an amount higher than the average order amount.

### Approach 1

<img width="746" height="342" alt="image" src="https://github.com/user-attachments/assets/202a3efe-1424-42c9-819a-aebd47b78a70" />

### Approach 2 – Using Window Functions

<img width="1092" height="488" alt="image" src="https://github.com/user-attachments/assets/385b995e-c136-4b73-91af-d673b6c20594" />

---

# Question 2 : Show details of orders whose amounts exceed the average order amount of their respective customers.

### Approach 1 – Using CTE

<img width="870" height="554" alt="image" src="https://github.com/user-attachments/assets/b106991a-d276-485a-8595-bf60f8b32063" />

### Approach 2 – Using Window Functions

<img width="1420" height="500" alt="image" src="https://github.com/user-attachments/assets/e748fca6-6f47-44dd-b2b2-5c61811611c8" />

---

# Question 3 : Based on the product price, what are the top 5 costliest product types within the 'Home & Furniture' category?

### Approach 1

<img width="1064" height="332" alt="image" src="https://github.com/user-attachments/assets/4c9d6d81-33ea-480e-b56e-9aa1dc103ff1" />

### Limitations

* Returns the top 5 rows instead of the top 5 ranked product types.
* Duplicate product types may appear in the result.
* Ties in price are not handled explicitly.
* Less flexible than window functions for ranking and Top-N analysis.

### Approach 2 – Using Window Functions

<img width="958" height="290" alt="image" src="https://github.com/user-attachments/assets/cdc41712-a901-4229-a251-1873983723de" />

---

# Question 4 : Display each customer's `customer_id`, `order_id`, `amount`, and their cumulative spending up to that order.

### Approach – Using Window Functions

<img width="1708" height="394" alt="image" src="https://github.com/user-attachments/assets/3616e3bf-1b2f-4e85-8ce4-81f9e1ff93ee" />

---

# Question 5 : Calculate the cumulative amount spent by each customer as orders are placed.

### Approach – Using Window Functions

<img width="2092" height="312" alt="image" src="https://github.com/user-attachments/assets/6ab2c586-9bd4-4b6e-82d9-9324b01aa7ce" />

---

# Question 6 : For each order, show the customer's total spending and the running total of spending in order of `order_date`.

### Approach – Using Window Functions

<img width="1986" height="238" alt="image" src="https://github.com/user-attachments/assets/c10a5af5-c63b-4a07-b21d-01bb61006e6f" />

---

# Question 7 : Display the moving sum of order amounts for each customer, with a window size of **N = 3**, ordered by their order dates.

### Window Frame Variations

**1. ROWS BETWEEN 2 PRECEDING AND CURRENT ROW**

**2. ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING**

**3. ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING**

### Approach – Using Window Functions

<img width="2104" height="450" alt="image" src="https://github.com/user-attachments/assets/1b0881a2-45bf-4091-9b2b-8a478e205b5a" />

---

# Question 8 : Calculate the cumulative sales amount where all transactions with the same sale amount should be treated together when computing the running total.

### Approach – Using Window Functions (`RANGE`)

```sql
SELECT
    order_id,
    amount,
    SUM(amount) OVER (
        ORDER BY amount
        RANGE BETWEEN UNBOUNDED PRECEDING
        AND CURRENT ROW
    ) AS running_sum
FROM `flipkart.orders`;
```

### Why `RANGE`?

* `RANGE` treats all rows having the same `amount` as a single group.
* Transactions with identical order amounts receive the same cumulative total.
* Unlike `ROWS`, duplicate values are processed together, making it ideal when ties should share the same running total.

<img width="1818" height="204" alt="image" src="https://github.com/user-attachments/assets/800c0e84-dd45-48c5-b2b3-45fe5f2e7f89" />
