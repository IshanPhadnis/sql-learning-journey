Problem Statement: Flipkart
Flipkart, one of India’s leading e-commerce platforms, wants to strengthen its competitive position by improving customer retention,
optimizing logistics, and increasing sales efficiency. With growing competition in the online retail space, the company’s leadership has
tasked the analytics team to leverage customer, product, and order data to uncover actionable insights.
The dataset contains information about customers, products, orders, and order items from January 2023 to September 2025.
	
		    	

Question 1 : Display customers who have placed orders with an amount higher than the average order amount.
Approach 1 - 
select *
from flipkart.customers c
join `flipkart.orders` o 
on c.customer_id = o.order_id
where o.amount>(select avg(amount)
                 from `flipkart.orders`)
order by o.amount desc;


Approach 2 using window functions



**Question 2 : Show details of orders whose amounts exceed the average order amount of their respective customers.**

Approach 1 using CTE-
with cust_avg_stats as (
select customer_id,
          avg(amount)avg_amt
  from `flipkart.orders`
  group by customer_id
  )

select *
from `flipkart.orders` o join cust_avg_stats ca
  on o.customer_id = ca.customer_id
join `flipkart.customers` c
  on o.customer_id = c.customer_id
ORDER BY o.amount DESC;

Approach 2 using Window Function -

with avg_stats as(select c.customer_id,c.first_name,c.last_name,o.amount,round(avg(o.amount) over(partition by c.customer_id),2)avg_amt

from flipkart.orders o  
join `flipkart.customers` c 
  on o.customer_id = c.customer_id
)
select *
from  avg_stats
where amount > avg_amt order by customer_id



Question 3 : Based on the product price, what are the top 5 costliest product types within the 'Home & Furniture' category?
Approach 1 - 

select p.price,
p.product_type
from `flipkart.order_items` o join `flipkart.products` p
on o.product_id = p.product_id
where p.category = 'Home & Furniture'
order by p.price
limit 5

### Limitations
- Returns the top 5 rows instead of the top 5 ranked product types.
- Duplicate product types may appear in the result.
- Ties in price are not handled explicitly.
- Less flexible than window functions for ranking and top-N analysis.


Approach 2 - 

select price,
product_type,
row_number() over(order by price desc) as row_no
from `flipkart.products`
where category = 'Home & Furniture'
limit 5










