# Uber Prob 1: Count active drivers & Accepted rides by month

## Problem Description & Tables
![1618951455970](https://user-images.githubusercontent.com/60673352/115581236-d1494980-a295-11eb-9223-736847df644b.jpg)
![image](https://user-images.githubusercontent.com/60673352/115581719-4452c000-a296-11eb-8b18-a6844392a079.png)

## Solution
```
# REPORT 1) the num of active drivers by the end of each 2020 month
#        2) the num of accepted rides in each 2020 month

# A) create month column from 1 to 12
WITH RECURSIVE month_col AS (SELECT 1 AS month
                             UNION ALL 
                             SELECT month + 1 FROM month_col WHERE month < 12),

# B) count active drivers by month of 2020
active_drivers AS (SELECT month, COUNT(driver_id) as active_drivers
                   FROM month_col m LEFT JOIN Drivers d
                 # count drivers number joined before each month of 2020
                   ON 202000 + m.month >= Date_format(join_date, "%Y%m")
                   GROUP BY month),

# C) count accepted rides by month of 2020
monthly_rides AS (SELECT MONTH(requested_at) AS month,
                  COUNT(ride_id) AS accepted_rides
                  FROM Rides JOIN acceptedRides USING (ride_id)
                  WHERE YEAR(requested_at) = '2020'
                  GROUP BY MONTH(requested_at))

# report active_drivers, accepted_rides of each 2020 month
SELECT ad.month,
       active_drivers,
       IFNULL(accepted_rides, 0) AS accepted_rides
FROM active_drivers ad LEFT JOIN monthly_rides USING(month)
```

## Summary
To calculate running sum, sometimes JOIN method is much better than the WINDOW function:
Count + JOIN with a comparison condition + Group by

**keys**:<br/>
1 some months have no orders, so I create a full month column(from 1 to 12) with Recursive CTE for the final report<br/>
*2(challenging) count the num of active drivers by the end of each month. Join drivers registered before that month, then count:
```
SELECT month, COUNT(d.driver_id) AS active_driver
FROM month_col m JOIN Drivers d
ON 202000 + m.month >= DATE_FORMAT(d.join_date, "%Y%m")
GROUP BY month
```
