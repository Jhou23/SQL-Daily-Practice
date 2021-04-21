## Problem Introduction
A telecommunications company wants to invest in new countries. <br/>
The company intends to invest in the countries where the **average call duration of the calls** in this country is **strictly greater than the global average **call duration.

Write an SQL query to find the countries where this company can invest.

## Tables
```Person table:
+----+----------+--------------+
| id | name     | phone_number |
+----+----------+--------------+
| 3  | Jonathan | 051-1234567  |
| 12 | Elvis    | 051-7654321  |
| 1  | Moncef   | 212-1234567  |
| 2  | Maroua   | 212-6523651  |
| 7  | Meir     | 972-1234567  |
| 9  | Rachel   | 972-0011100  |
+----+----------+--------------+

Country table:
+----------+--------------+
| name     | country_code |
+----------+--------------+
| Peru     | 051          |
| Israel   | 972          |
| Morocco  | 212          |
| Germany  | 049          |
| Ethiopia | 251          |
+----------+--------------+

Calls table:
+-----------+-----------+----------+
| caller_id | callee_id | duration |
+-----------+-----------+----------+
| 1         | 9         | 33       |
| 2         | 9         | 4        |
| 1         | 2         | 59       |
| 3         | 12        | 102      |
| 3         | 12        | 330      |
| 12        | 3         | 5        |
| 7         | 9         | 13       |
| 7         | 1         | 3        |
| 9         | 7         | 1        |
| 1         | 7         | 7        |
+-----------+-----------+----------+

Result table:
+----------+
| country  |
+----------+
| Peru     |
+----------+
The average call duration for Peru is (102 + 102 + 330 + 330 + 5 + 5) / 6 = 145.666667
The average call duration for Israel is (33 + 4 + 13 + 13 + 3 + 1 + 1 + 7) / 8 = 9.37500

Since Peru is the only country where average call duration is greater than the global average, it's the only recommended country.
```

## Code
```
    # get the global avg call duration 
    WITH AVG_duration AS (SELECT AVG(duration) duration FROM Calls)

    # compare country avg call duration with global
    SELECT c.name AS country
    FROM Calls, Person, Country c
    # avoid UNION two tables join with the same table seperately with WHERE(exp OR exp)
    WHERE (caller_id = id OR callee_id = id) AND country_code = LEFT(phone_number, 3)
    GROUP BY country_code
    HAVING AVG(duration) > (SELECT duration FROM AVG_duration);
```

## Summary
When I want to join a table to another one twice, and we want to get a long column with value such as sales and units.<br/>
1. Use WHERE(id = t1.col1 OR t1.col2) to avoid UNION two tables when a table needs to join another twice.

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
