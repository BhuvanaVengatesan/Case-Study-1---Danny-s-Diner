# Case Study #1 - Danny's Diner

<img src="https://github.com/BhuvanaVengatesan/Danny-s-Diner-SQL-Challenges/assets/172362151/e31bcf7f-71cf-4963-8bfa-41a170d259bc" width="300">

View the case study [here](https://8weeksqlchallenge.com/case-study-1/)

# Entity Relationship Diagram

<img src="https://user-images.githubusercontent.com/81607668/127271130-dca9aedd-4ca9-4ed8-b6ec-1e1920dca4a8.png" width="600">

# Introduction

Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

# Datasets used
* sales: The sales table captures all customer_id level purchases with an corresponding order_date and product_id information for when and what menu items were ordered.
* menu: The menu table maps the product_id to the actual product_name and price of each menu item.
* members: The members table captures the join_date when a customer_id joined the beta version of the Danny’s Diner loyalty program.

# Case Study Questions
1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

# Solution
## 1. What is the total amount each customer spent at the restaurant?

```
SELECT s.customer_id , CONCAT('$', (SUM(m.price))) AS total_sales
FROM sales s INNER JOIN menu m
ON s.product_id=m.product_id
GROUP BY customer_id;
```

![image](https://github.com/BhuvanaVengatesan/Danny-s-Diner-SQL-Challenges/assets/172362151/e2a265b4-5172-478b-8edf-72740cd883d3)

## 2. How many days has each customer visited the restaurant?
```
SELECT customer_id, COUNT(DISTINCT(order_date)) AS days_visited FROM sales
GROUP BY customer_id;
```
![image](https://github.com/BhuvanaVengatesan/Danny-s-Diner-SQL-Challenges/assets/172362151/7afb7197-bc62-4910-9448-687f82cf02d7)

## 3. What was the first item from the menu purchased by each customer?
```
WITH cte AS(
SELECT s.customer_id, m.product_name, s.order_date, 
DENSE_RANK() OVER(PARTITION BY s.customer_id ORDER BY s.order_date) AS rank1 FROM sales s INNER JOIN menu m
ON s.product_id=m.product_id
GROUP BY  s.customer_id, m.product_name, s.order_date
)
SELECT customer_id, product_name FROM cte
WHERE rank1=1;
```
![image](https://github.com/BhuvanaVengatesan/Danny-s-Diner-SQL-Challenges/assets/172362151/dcf2cd3e-a3f7-4c1d-b670-b03dbb64a811)

## 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
```
SELECT m.product_name, COUNT(s.product_id) AS ordered FROM sales AS s 
INNER JOIN menu AS m
ON  s.product_id=m.product_id
GROUP BY m.product_name
ORDER BY COUNT(s.product_id) DESC
LIMIT 1;
```
![image](https://github.com/BhuvanaVengatesan/Danny-s-Diner-SQL-Challenges/assets/172362151/adbc6ea0-0075-46fe-ada8-7f24b6c5a741)



