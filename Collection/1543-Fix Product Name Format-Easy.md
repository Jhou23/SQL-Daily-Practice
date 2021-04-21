## Problem Introduction

Since table Sales was filled manually in the year 2000, product_name may contain leading and/or trailing white spaces, also they are case-insensitive.

Write an SQL query to report

- product_name in lowercase without leading or trailing white spaces.
- sale_date in the format ('YYYY-MM').
- total the number of times the product was sold in this month.

## Tables
```
Sales
+---------+--------------+------------+
| sale_id | product_name | sale_date  |
+---------+--------------+------------+
| 1       | LCPHONE      | 2000-01-16 |
| 2       | LCPhone      | 2000-01-17 |
| 3       | LcPhOnE      | 2000-02-18 |
| 4       | LCKeyCHAiN   | 2000-02-19 |
| 5       | LCKeyChain   | 2000-02-28 |
| 6       | Matryoshka   | 2000-03-31 |
+---------+--------------+------------+

Result table:
+--------------+-----------+-------+
| product_name | sale_date | total |
+--------------+-----------+-------+
| lckeychain   | 2000-02   | 2     |
| lcphone      | 2000-01   | 2     |
| lcphone      | 2000-02   | 1     |
| matryoshka   | 2000-03   | 1     |
+--------------+-----------+-------+
In January, 2 LcPhones were sold, please note that the product names are not case sensitive and may contain spaces.
In Februery, 2 LCKeychains and 1 LCPhone were sold.
In March, 1 matryoshka was sold.
```

## Code
```
    SELECT LOWER(TRIM(product_name)) AS product_name, 
           LEFT(sale_date, 7) AS sale_date, 
           COUNT(product_name) AS total
    FROM Sales
    GROUP BY LOWER(TRIM(product_name)), LEFT(sale_date, 7)
    ORDER BY LOWER(TRIM(product_name)), LEFT(sale_date, 7)
```

## Summary
trim() trim the spaces on both sides
ltrim() trim the left spaces
rtrim() trim the right spaces
replace(... , ' ', '') delete all spaces
lower() upper()

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
