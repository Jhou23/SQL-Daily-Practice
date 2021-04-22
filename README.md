# SQL Problems Collection

This repository is the log of my daily sql practice. I hope it can be of some help for you.


## Introduction

Each problem typically has four parts: 
1. Goal of Query
2. Table Introduction
3. Solution
4. Summary

I would recommend you read the problem first, understand the tables and relationship between tables, and think about solutions first.
My solution is a reference, and Summary is about my methods to solve the problem, my understanding of the SQL function use cases.

Happy Reading!

## Feedback

If you have any suggestions to the collections such as the format of sharing and better solution, I am happy to speak with you.

**Email**   : jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898



## Example
  ### Uber Prob 1: Count active drivers & Accepted rides by month

  ### Problem Description & Tables
  ```
  Write an SQL query to report the following statistics for each month of 2020:

  The number of drivers currently with the Hopper company by the end of the month (active_drivers).
  - The number of accepted rides in that month (accepted_rides).
  - Return the result table ordered by month in ascending order, where month is the month's number (January is 1, February is 2, etc.).

  Drivers table:
  +-----------+------------+
  | driver_id | join_date  |
  +-----------+------------+
  | 10        | 2019-12-10 |
  | 8         | 2020-1-13  |
  | 5         | 2020-2-16  |
  | 7         | 2020-3-8   |
  | 4         | 2020-5-17  |
  | 1         | 2020-10-24 |
  | 6         | 2021-1-5   |
  +-----------+------------+

  Rides table:
  +---------+---------+--------------+
  | ride_id | user_id | requested_at |
  +---------+---------+--------------+
  | 6       | 75      | 2019-12-9    |
  | 1       | 54      | 2020-2-9     |
  | 10      | 63      | 2020-3-4     |
  | 19      | 39      | 2020-4-6     |
  | 3       | 41      | 2020-6-3     |
  | 13      | 52      | 2020-6-22    |
  | 7       | 69      | 2020-7-16    |
  | 17      | 70      | 2020-8-25    |
  | 20      | 81      | 2020-11-2    |
  | 5       | 57      | 2020-11-9    |
  | 2       | 42      | 2020-12-9    |
  | 11      | 68      | 2021-1-11    |
  | 15      | 32      | 2021-1-17    |
  | 12      | 11      | 2021-1-19    |
  | 14      | 18      | 2021-1-27    |
  +---------+---------+--------------+

  AcceptedRides table:
  +---------+-----------+---------------+---------------+
  | ride_id | driver_id | ride_distance | ride_duration |
  +---------+-----------+---------------+---------------+
  | 10      | 10        | 63            | 38            |
  | 13      | 10        | 73            | 96            |
  | 7       | 8         | 100           | 28            |
  | 17      | 7         | 119           | 68            |
  | 20      | 1         | 121           | 92            |
  | 5       | 7         | 42            | 101           |
  | 2       | 4         | 6             | 38            |
  | 11      | 8         | 37            | 43            |
  | 15      | 8         | 108           | 82            |
  | 12      | 8         | 38            | 34            |
  | 14      | 1         | 90            | 74            |
  +---------+-----------+---------------+---------------+

  Result table:
  +-------+----------------+----------------+
  | month | active_drivers | accepted_rides |
  +-------+----------------+----------------+
  | 1     | 2              | 0              |
  | 2     | 3              | 0              |
  | 3     | 4              | 1              |
  | 4     | 4              | 0              |
  | 5     | 5              | 0              |
  | 6     | 5              | 1              |
  | 7     | 5              | 1              |
  | 8     | 5              | 1              |
  | 9     | 5              | 0              |
  | 10    | 6              | 0              |
  | 11    | 6              | 2              |
  | 12    | 6              | 1              |
  +-------+----------------+----------------+

  By the end of January --> two active drivers (10, 8) and no accepted rides.
  By the end of February --> three active drivers (10, 8, 5) and no accepted rides.
  ```

  ### Solution
    
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
