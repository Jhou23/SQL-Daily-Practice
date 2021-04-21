# Find the Missing IDs

## Problem Introduction
The missing IDs are ones that are not in the Customers table but are in the range between 1 and the maximum customer_id present in the table. The customer_id is in ascending order.
```
For example:
Customer_id
1
2
4
5
8
Missing ids are 3 6 7
```

## Code
```
WITH RECURSIVE num(n) AS (
SELECT 1 AS n
UNION ALL
SELECT n + 1 FROM num WHERE n < (SELECT MAX(customer_id) FROM Customers)
)

SELECT n as ids
FROM num LEFT JOIN Customers c ON num.n = c.customer_id
WHERE customer_id IS NULL
```

## Summary
recursive common table expressions (CTEs) are great for series generation and traversing hierarchical data

## Resource
Recursive CTE document: https://lnkd.in/gmyue4Q
Recursive CTE article: https://lnkd.in/gN27742

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
