## Problem Introduction
Write an SQL query to report the Total sales amount of each item for each year, with corresponding product name, product_id, product_name and report_year.

Dates of the sales years are between 2018 to 2020. Return the result table ordered by product_id and report_year.

## Tables
```
Product table:
+------------+--------------+
| product_id | product_name |
+------------+--------------+
| 1          | LC Phone     |
| 2          | LC T-Shirt   |
| 3          | LC Keychain  |
+------------+--------------+

Sales table:
+------------+--------------+-------------+---------------------+
| product_id | period_start | period_end  | average_daily_sales |
+------------+--------------+-------------+---------------------+
| 1          | 2019-01-25   | 2019-02-28  | 100                 |
| 2          | 2018-12-01   | 2020-01-01  | 10                  |
| 3          | 2019-12-01   | 2020-01-31  | 1                   |
+------------+--------------+-------------+---------------------+

Result table:
+------------+--------------+-------------+--------------+
| product_id | product_name | report_year | total_amount |
+------------+--------------+-------------+--------------+
| 1          | LC Phone     |    2019     | 3500         |
| 2          | LC T-Shirt   |    2018     | 310          |
| 2          | LC T-Shirt   |    2019     | 3650         |
| 2          | LC T-Shirt   |    2020     | 10           |
| 3          | LC Keychain  |    2019     | 31           |
| 3          | LC Keychain  |    2020     | 31           |
+------------+--------------+-------------+--------------+
LC Phone was sold for the period of 2019-01-25 to 2019-02-28, and there are 35 days for this period. Total amount 35*100 = 3500. 
LC T-shirt was sold for the period of 2018-12-01 to 2020-01-01, and there are 31, 365, 1 days for years 2018, 2019 and 2020 respectively.
LC Keychain was sold for the period of 2019-12-01 to 2020-01-31, and there are 31, 31 days for years 2019 and 2020 respectively.
```

## Code
```
    SELECT  t.product_id, p.product_name, t.report_year, t.total_amount
    FROM
        (SELECT
            product_id, '2020' report_year,
            (DATEDIFF(if(period_end < '2021-01-01', period_end, '2020-12-31'),
            if(period_start > '2019-12-31', period_start, '2020-01-01'))+1)*average_daily_sales AS total_amount
        FROM Sales
        HAVING total_amount > 0
        UNION ALL
        SELECT
            product_id, '2019' report_year,
            (DATEDIFF(if(period_end < '2020-01-01', period_end, '2019-12-31'),
            if(period_start > '2018-12-31', period_start, '2019-01-01'))+1)*average_daily_sales AS total_amount
        FROM Sales
        HAVING total_amount > 0
        UNION ALL
        SELECT
            product_id, '2018' report_year,
            (DATEDIFF(if(period_end < '2019-01-01', period_end, '2018-12-31'),
            if(period_start > '2017-12-31', period_start, '2018-01-01'))+1)*average_daily_sales AS total_amount
        FROM Sales
        HAVING total_amount > 0) t
        LEFT JOIN Product p ON p.product_id = t.product_id
    GROUP BY t.product_id, t.report_year
    ORDER BY t.product_id, t.report_year
```

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
