# Restaurant-Orders-Analysis

## **Restaurant Orders Report**

# Project Overview

This project aims to analyze restaurant order datasets using SQL and visualization tools to derive insights into customer behavior, popular menu items, peak hours, and operational efficiencies. Key analyses include identifying top-selling and least selling items, understanding customer ordering patterns, evaluating sales trends, and optimizing promotional strategies and development of most preferred cuisine. The datasets utilized encompass orders, menu items, orders information, and price enabling comprehensive data-driven decision-making in restaurant operations


![Dashboard](https://github.com/Xharity/Restaurant-Sales-Analysis/assets/173803485/a73ad9b0-ea34-4f20-8709-ee9969423878)

Dashboard Link ()

## Data Sources

The dataset used for this analysis is the restaurant dataset file containing information about the restaurant order details made by the company 

## Tools

- Excel [download here] (link)
- SQL
- PowerBI
     - [Dashboard Link](https://github.com/Xharity/Restaurant-Sales-Analysis/assets/173803485/a73ad9b0-ea34-4f20-8709-ee9969423878)

# Data Cleaning/ preparation

In the initial data preparation phase, we performed the following tasks

1. Data loading and inspection 
2. Data transformation and merging tables 
3. Data cleaning and formatting 



# *Business Questions*

1.What were the least and most ordered items? What categories were they in?

2.What do the highest spend orders look like? Which items did they buy and how much did they spend?

3. Were there certain times that had more or less orders?

4. Which cuisines should we focus on developing more menu items for based on the data?

## Data Analysis using SQL 

1. View the menu_items table and write a query to find the number of items on the menu

```select count(menu_item_id) as "total no of items" from menu_items```

2. What are the least and most expensive items on the menu 

```SELECT TOP 1 item_name, price FROM menu_items ORDER BY price DESC```

3. How many Italian dishes are on the menu

`select count(item_name) as 'no of dishes', category from menu_items 
where category like 'ITA%'
Group by category`

4. What are the least and most expensive Italian dishes on the menu

` select top 1 item_name, category from menu_items
 where category like 'ITA%'
 order by price desc`

 `select top 1 item_name, category from menu_items
 where category like 'ITA%'
 order by price asc`
 
5. How many dishes are in each category? What is the average dish price within each category

`SELECT COUNT(item_name) AS dish_count, category
FROM menu_items
GROUP BY category`

`SELECT category, ROUND(AVG(price),2)  AS Average_dish_price FROM menu_items
GROUP  BY category
ORDER BY AVG(price) DESC`

6. View the order_details table. What is the date range of the table?

`SELECT * FROM order_details`

`SELECT MIN(order_date) AS start_date, MAX(order_date) AS end_date FROM order_details`

7. How many orders were made within this date range?

`SELECT DISTINCT COUNT(order_id) AS Total_Order FROM order_details`

8. How many items were ordered within this date range?

`SELECT menu_item_id, COUNT(item_id)
FROM menu_items m
JOIN order_details o
ON m.menu_item_id = o.item_id
GROUP BY menu_item_id`

`SELECT COUNT(item_id) AS Total_item_ordered
FROM order_details`

9.  Which orders had the most number of items?

`SELECT order_id, COUNT(item_id) as 'item_count'
FROM order_details
GROUP BY order_id
ORDER BY COUNT(item_id) DESC`

10. How many orders had more than 12 items?

`SELECT order_id, COUNT(item_id)
FROM order_details
GROUP BY order_id
HAVING COUNT(item_id) > 12
ORDER BY COUNT(item_id) DESC`

`SELECT SUM(sub.total_orders) as 'total_orders_above_12'
FROM (
    SELECT COUNT(order_id) as 'total_orders'
    FROM order_details
    GROUP BY order_id
    HAVING COUNT(*) > 12
) as sub`

11. Combine the menu_items and order_details tables into a single table

`SELECT * FROM 
order_details o
INNER JOIN menu_items m
ON m.menu_item_id = o.item_id`

12. What were the least and most ordered items? What categories were they in?
Which menu are doing well?

`SELECT top 5 m.category, m.item_name, COUNT(o.order_id) 
FROM menu_items m
JOIN order_details o
ON m.menu_item_id = o.item_id
GROUP BY m.category, m.item_name
ORDER BY COUNT(order_id) ASC`

`SELECT Top 5 m.category, m.item_name, COUNT(o.order_id) 
FROM menu_items m
JOIN order_details o
ON m.menu_item_id = o.item_id
GROUP BY m.category, m.item_name
ORDER BY COUNT(order_id) DESC

13. What were the top 5 orders that spent the most money?

`SELECT  top 5 o.order_id, SUM(m.price) Total_Spent
FROM menu_items m
JOIN order_details o
ON m.menu_item_id = o.item_id
GROUP BY o.order_id 
ORDER BY Total_Spent DESC`

14. View the details of the highest spend order. Which specific items were purchased?

`SELECT category, COUNT(item_id) AS Number_of_Items
FROM menu_items m
JOIN order_details o
ON m.menu_item_id = o.item_id
WHERE order_id = 440
GROUP BY category`

15. View the details of the top 5 highest spend orders

`SELECT order_id, category, COUNT(item_id) AS Number_of_Items
FROM menu_items m
JOIN order_details o
ON m.menu_item_id = o.item_id
WHERE order_id IN(440,2075,1957,330,2675)
GROUP BY category, order_id`
