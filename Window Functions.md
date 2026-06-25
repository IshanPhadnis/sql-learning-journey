Problem Statement: Flipkart
Flipkart, one of India’s leading e-commerce platforms, wants to strengthen its competitive position by improving customer retention,
optimizing logistics, and increasing sales efficiency. With growing competition in the online retail space, the company’s leadership has
tasked the analytics team to leverage customer, product, and order data to uncover actionable insights.
The dataset contains information about customers, products, orders, and order items from January 2023 to September 2025.
	
		    	

**Question 1 : Display customers who have placed orders with an amount higher than the average order amount.**

Approach 1 - 

<img width="746" height="342" alt="image" src="https://github.com/user-attachments/assets/202a3efe-1424-42c9-819a-aebd47b78a70" />

Approach 2 using window functions - 
<img width="1092" height="488" alt="image" src="https://github.com/user-attachments/assets/385b995e-c136-4b73-91af-d673b6c20594" />

**Question 2 : Show details of orders whose amounts exceed the average order amount of their respective customers.**

Approach 1 using CTE -
<img width="870" height="554" alt="image" src="https://github.com/user-attachments/assets/b106991a-d276-485a-8595-bf60f8b32063" />

Approach 2 using Window Function -
<img width="1420" height="500" alt="image" src="https://github.com/user-attachments/assets/e748fca6-6f47-44dd-b2b2-5c61811611c8" />

**Question 3 : Based on the product price, what are the top 5 costliest product types within the 'Home & Furniture' category?**

Approach 1 - 
<img width="1064" height="332" alt="image" src="https://github.com/user-attachments/assets/4c9d6d81-33ea-480e-b56e-9aa1dc103ff1" />

### Limitations
- Returns the top 5 rows instead of the top 5 ranked product types.
- Duplicate product types may appear in the result.
- Ties in price are not handled explicitly.
- Less flexible than window functions for ranking and top-N analysis.

Approach 2 - 
<img width="958" height="290" alt="image" src="https://github.com/user-attachments/assets/cdc41712-a901-4229-a251-1873983723de" />










