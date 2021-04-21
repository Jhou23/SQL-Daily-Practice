## Problem Introduction
Write an SQL query to report all possible axis-aligned rectangles with non-zero area that can be formed by any two points in the Points table.

Each row in the result should contain three columns (p1, p2, area) where:

p1 and p2 are the id's of the two points that determine the opposite corners of a rectangle.
area is the area of the rectangle and must be non-zero.
Report the query in descending order by area first, then in ascending order by p1's id if there is a tie, then in ascending order by p2's id if there is another tie.

## Tables
```
Points table:
+----------+-------------+-------------+
| id       | x_value     | y_value     |
+----------+-------------+-------------+
| 1        | 2           | 7           |
| 2        | 4           | 8           |
| 3        | 2           | 10          |
+----------+-------------+-------------+

Result table:
+----------+-------------+-------------+
| p1       | p2          | area        |
+----------+-------------+-------------+
| 2        | 3           | 4           |
| 1        | 2           | 2           |
+----------+-------------+-------------+

![image](https://user-images.githubusercontent.com/60673352/115613245-29924280-a2ba-11eb-8655-55c62f919cc8.png)
The rectangle formed by p1 = 2 and p2 = 3 has an area equal to |4-2| * |8-10| = 4.
The rectangle formed by p1 = 1 and p2 = 2 has an area equal to |2-4| * |7-8| = 2.
Note that the rectangle formed by p1 = 1 and p2 = 3 is invalid because the area is 0.
```

## Code
```
SELECT p1.id AS p1, p2.id AS p2, 
       ABS((p1.x_value - p2.x_value)*(p1.y_value - p2.y_value)) AS `WHERE`
FROM Points p1, Points p2
WHERE p1.id < p2.id AND (p1.x_value-p2.x_value)*(p1.y_value-p2.y_value) <> 0
ORDER BY `WHERE` DESC, p1, p2
```

## Summary
```
general case - get all the pairs of id without duplicates:
for example, we want to compare the salary between all employees. SELF JOIN + WHERE clause could be the solution.
SELECT id FROM table t1, table t2 WHERE t1.id < t2.id
```

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
