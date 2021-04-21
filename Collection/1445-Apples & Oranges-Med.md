## Problem Introduction
Write an SQL query to report the difference between number of apples and oranges sold each day.

Return the result table ordered by sale_date in format ('YYYY-MM-DD').

## Tables
```
Sales table:
+------------+------------+-------------+
| sale_date  | fruit      | sold_num    |
+------------+------------+-------------+
| 2020-05-01 | apples     | 10          |
| 2020-05-01 | oranges    | 8           |
| 2020-05-02 | apples     | 15          |
| 2020-05-02 | oranges    | 15          |
| 2020-05-03 | apples     | 20          |
| 2020-05-03 | oranges    | 0           |
| 2020-05-04 | apples     | 15          |
| 2020-05-04 | oranges    | 16          |
+------------+------------+-------------+

Result table:
+------------+--------------+
| sale_date  | diff         |
+------------+--------------+
| 2020-05-01 | 2            |
| 2020-05-02 | 0            |
| 2020-05-03 | 20           |
| 2020-05-04 | -1           |
+------------+--------------+

Day 2020-05-01, 10 apples and 8 oranges were sold (Difference  10 - 8 = 2).
Day 2020-05-02, 15 apples and 15 oranges were sold (Difference 15 - 15 = 0).
Day 2020-05-03, 20 apples and 0 oranges were sold (Difference 20 - 0 = 20).
Day 2020-05-04, 15 apples and 16 oranges were sold (Difference 15 - 16 = -1).
```

## Code
```
    # Method 1: basic join and deduct
    SELECT a.sale_date, a.sold_num - o.sold_num AS diff
    FROM (SELECT * FROM Sales WHERE fruit = 'apples') a
         JOIN (SELECT * FROM Sales WHERE fruit = 'oranges') o
           ON a.sale_date = o.sale_date
    ORDER BY a.sales_date

    # Method 2: GROUP BY with IF
    # set apple as negative sold_num and then sum by date
    SELECT sale_date,
    SUM(IF(fruit = 'apples', sold_num, -sold_num)) diff
    FROM Sales
    GROUP BY sale_date
    ORDER BY sale_date
```

## Summary
Combine aggregation functions with IF/CASE WHEN to avoid more joins.

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
