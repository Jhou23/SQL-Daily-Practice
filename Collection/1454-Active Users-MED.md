## Problem Introduction
Write an SQL query to find the id and the name of active users.

Active users are those who logged in to their accounts for 5 or more consecutive days.

Return the result table ordered by the id.

## Tables
```
Accounts table:
+----+----------+
| id | name     |
+----+----------+
| 1  | Winston  |
| 7  | Jonathan |
+----+----------+

Logins table:
+----+------------+
| id | login_date |
+----+------------+
| 7  | 2020-05-30 |
| 1  | 2020-05-30 |
| 7  | 2020-05-31 |
| 7  | 2020-06-01 |
| 7  | 2020-06-02 |
| 7  | 2020-06-02 |
| 7  | 2020-06-03 |
| 1  | 2020-06-07 |
| 7  | 2020-06-10 |
+----+------------+

Result table:
+----+----------+
| id | name     |
+----+----------+
| 7  | Jonathan |
+----+----------+
User Winston with id = 1 logged in 2 times only in 2 different days, so, Winston is not an active user.
User Jonathan with id = 7 logged in 7 times in 6 different days, five of them were consecutive days, so, Jonathan is an active user.
```

## Code
```
    # Method 2: # Get active users who logged in for more than 5 days
    # 1.get unique id, login_date pair since users might log in more than once per day
    WITH daily_login AS (
        SELECT id, login_date
        FROM Logins
        GROUP BY id, login_date),

    # 2. divide login_date date into consecutive groups 
    #   2a. attach row number to user's login date 
    rowNumber AS (
        SELECT id, login_date, ROW_NUMBER() OVER(PARTITION BY id ORDER BY login_date) AS rn  
        FROM daily_login),
    #   2b. Subtract row number from login_date, when subtract consectivce row numbers from login_date, if login_dates are consecutive, the difference will be the same; 
    # SUBDATE('5/01', 1) = '4/30'; SUBDATE('5/02', 2) = '4/30'
    consecutive_login_dates AS (
        SELECT id, SUBDATE(login_date, rn) as consecutive_dates_signal
        FROM rowNumber),
    #   2c. get active users with > 5 rows after grouping by id, consecutive_dates_signal
    active_user AS (
        SELECT id
        FROM consecutive_login_dates
        GROUP BY id, consecutive_dates_signal
        HAVING COUNT(id) >= 5)

    # 3. get the distinct name and id of active users, 
    #    since one user may have more than one 5-days-consecutive logins
        SELECT DISTINCT u.id, a.name
        FROM active_user u
        JOIN Accounts a ON a.id = u.id
        ORDER BY id
```

## Summary
```
Methods: 
divide consecutive login dates of each user into different groups and see which group has more than 5 consecutive login dates. 
Then we know which user is an active user.

Steps:
A. attach consecutive row numbers to each users records
user_id | login_date | row_num
1 5/1 1
1 5/2 2
1 5/5 3

B. get the consecutive dates by subtracting row_number from login_date. consecutive login dates have same diff
user_id | login_date | row_num | diff
1 5/1 1 4/30
1 5/2 2 4/30
1 5/5 3 5/2

C. get active users by GROUP BY user_id, diff to find count(rows) > 5
```

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
