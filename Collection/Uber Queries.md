# Uber Prob 1: Count active drivers & Accepted rides by month

## Problem Description & Tables
![1618951455970](https://user-images.githubusercontent.com/60673352/115581236-d1494980-a295-11eb-9223-736847df644b.jpg)
![image](https://user-images.githubusercontent.com/60673352/115581719-4452c000-a296-11eb-8b18-a6844392a079.png)

## Solution
![image](https://user-images.githubusercontent.com/60673352/115582828-52551080-a297-11eb-8e61-5f2684435f4a.png)

## Summary
To calculate running sum, sometimes JOIN method is much better than the WINDOW function:
Count + JOIN with a comparison condition + Group by

keys:
1 some months have no orders, so I create a full month column(from 1 to 12) with Recursive CTE for the final report

*2(challenging) count the num of active drivers by the end of each month. Join drivers registered before that month, then count:

```
{
SELECT month, COUNT(d.driver_id) AS active_driver
FROM month_col m JOIN Drivers d
ON 202000 + m.month >= DATE_FORMAT(d.join_date, "%Y%m")
GROUP BY month
}
```
