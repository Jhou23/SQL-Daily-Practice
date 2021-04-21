## Problem Introduction
You are the business owner and would like to obtain a sales report for category items and day of the week.

Write an SQL query to report how many units in each category have been ordered on each day of the week.

Return the result table ordered by category.

## Tables
```
Orders table:
+------------+--------------+-------------+--------------+-------------+
| order_id   | customer_id  | order_date  | item_id      | quantity    |
+------------+--------------+-------------+--------------+-------------+
| 1          | 1            | 2020-06-01  | 1            | 10          |
| 2          | 1            | 2020-06-08  | 2            | 10          |
| 3          | 2            | 2020-06-02  | 1            | 5           |
| 4          | 3            | 2020-06-03  | 3            | 5           |
| 5          | 4            | 2020-06-04  | 4            | 1           |
| 6          | 4            | 2020-06-05  | 5            | 5           |
| 7          | 5            | 2020-06-05  | 1            | 10          |
| 8          | 5            | 2020-06-14  | 4            | 5           |
| 9          | 5            | 2020-06-21  | 3            | 5           |
+------------+--------------+-------------+--------------+-------------+

Items table:
+------------+----------------+---------------+
| item_id    | item_name      | item_category |
+------------+----------------+---------------+
| 1          | LC Alg. Book   | Book          |
| 2          | LC DB. Book    | Book          |
| 3          | LC SmarthPhone | Phone         |
| 4          | LC Phone 2020  | Phone         |
| 5          | LC SmartGlass  | Glasses       |
| 6          | LC T-Shirt XL  | T-Shirt       |
+------------+----------------+---------------+

Result table:
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
| Category   | Monday    | Tuesday   | Wednesday | Thursday  | Friday    | Saturday  | Sunday    |
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
| Book       | 20        | 5         | 0         | 0         | 10        | 0         | 0         |
| Glasses    | 0         | 0         | 0         | 0         | 5         | 0         | 0         |
| Phone      | 0         | 0         | 5         | 1         | 0         | 0         | 10        |
| T-Shirt    | 0         | 0         | 0         | 0         | 0         | 0         | 0         |
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
On Monday (2020-06-01, 2020-06-08) were sold a total of 20 units (10 + 10) in the category Book (ids: 1, 2).
On Tuesday (2020-06-02) were sold a total of 5 units  in the category Book (ids: 1, 2).
On Wednesday (2020-06-03) were sold a total of 5 units in the category Phone (ids: 3, 4).
On Thursday (2020-06-04) were sold a total of 1 unit in the category Phone (ids: 3, 4).
On Friday (2020-06-05) were sold 10 units in the category Book (ids: 1, 2) and 5 units in Glasses (ids: 5).
On Saturday there are no items sold.
On Sunday (2020-06-14, 2020-06-21) were sold a total of 10 units (5 +5) in the category Phone (ids: 3, 4).
There are no sales of T-Shirt.
```

## Code
```
# report the amount of units sold on each day of week
# 1.get the category info of each order, extract day of week of dates
# 2.aggeragate daily sales of each category
WITH unit_by_category_and_weekday AS (
    SELECT item_category,DAYOFWEEK(order_date) AS day_of_week, 
           SUM(quantity) AS quantity_by_day
    FROM Orders o LEFT JOIN Items i USING(item_id)
    GROUP BY item_category, DAYOFWEEK(order_date)
    ),

# 3. get the distinct list of categories for calculate sales of them
category_list AS (
    SELECT DISTINCT item_category AS categories
    FROM Items)

# 4. convert values of weekday column(monday to sunday) to 7 columns
SELECT categories AS Category,
       MAX(IF(day_of_week = 2, quantity_by_day, 0)) Monday,
       MAX(IF(day_of_week = 3, quantity_by_day, 0)) Tuesday,
       MAX(IF(day_of_week = 4, quantity_by_day, 0)) Wednesday,
       MAX(IF(day_of_week = 5, quantity_by_day, 0)) Thursday,
       MAX(IF(day_of_week = 6, quantity_by_day, 0)) Friday,
       MAX(IF(day_of_week = 7, quantity_by_day, 0)) Saturday,
       MAX(IF(day_of_week = 1, quantity_by_day, 0)) Sunday
FROM category_list c LEFT JOIN unit_by_category_and_weekday u 
     ON c.categories = u.item_category
GROUP BY categories
ORDER BY categories
```

## Summary
```
convert long table to wide table, for example:

category | dayofweek | quantity
book Monday 50
glasses Tuesday 40

to

category | Mon | Tue | Wed | Thu | Fri | Sat | Sun
book 50 0 ....
glasses 0 40 ....

General Solution for long to wide:

SELECT category,
MAX(IF(dayofweek = 'Mon', quantity, 0))
# the same method for Tue to Sun columns
FROM table
GROUP BY category

General Solution for wide to long. UNION
SELECT category, 'Monday', Mon
FROM table
UNION
SELECT category, 'Monday', TUE
FROM table
..... # UNION all the tables
```

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
