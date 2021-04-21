# "Favorite" Product by Customer

## Problem Introduction
Find the most frequently ordered product(s) for each customer

## Table
```
Orders
+----------+------------+-------------+------------+
| order_id | order_date | customer_id | product_id |
+----------+------------+-------------+------------+
| 1        | 2020-07-31 | 1           | 1          |
| 2        | 2020-07-30 | 2           | 2          |
| 3        | 2020-08-29 | 3           | 3          |
| 4        | 2020-07-29 | 4           | 1          |
| 5        | 2020-06-10 | 1           | 2          |
| 6        | 2020-08-01 | 2           | 1          |
| 7        | 2020-08-01 | 3           | 3          |
| 8        | 2020-08-03 | 1           | 2          |
| 9        | 2020-08-07 | 2           | 3          |
| 10       | 2020-07-15 | 1           | 2          |
+----------+------------+-------------+------------+

Products
+------------+--------------+-------+
| product_id | product_name | price |
+------------+--------------+-------+
| 1          | keyboard     | 120   |
| 2          | mouse        | 80    |
| 3          | screen       | 600   |
| 4          | hard disk    | 450   |
+------------+--------------+-------+

Result table:
+-------------+------------+--------------+
| customer_id | product_id | product_name |
+-------------+------------+--------------+
| 1           | 2          | mouse        |
| 2           | 1          | keyboard     |
| 2           | 2          | mouse        |
| 2           | 3          | screen       |
| 3           | 3          | screen       |
| 4           | 1          | keyboard     |
+-------------+------------+--------------+
```

## Code
```
    # Count the frequency of each product by customer_id
    WITH frequency_of_product AS (
        SELECT customer_id, product_id, COUNT(product_id) AS frequency
        FROM Orders 
        GROUP BY customer_id, product_id),

    # Dense rank the product frequency
    product_rank AS (
        SELECT *, DENSE_RANK() OVER(PARTITION BY customer_id ORDER BY frequency DESC) rnk
        FROM frequency_of_product
    )

    # Select the favorite product by customer
    # Result could have >1 products because of the same frequencies
    SELECT r.customer_id, r.product_id, p.product_name
    FROM product_rank r JOIN Products p USING(product_id)
    WHERE rnk = 1
    ORDER BY customer_id
```

## Summary
Method:<br/>
To know the product most frequently bought -> <br/>
I want to rank the frequency of products bought by a customer -> <br/>
I need to count the frequency of products by customer 

Code steps:
count frequency -> rank frequency -> select product whose rank = 1 for each customer

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
