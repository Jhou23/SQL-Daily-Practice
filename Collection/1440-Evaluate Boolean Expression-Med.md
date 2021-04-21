## Problem Introduction
Write an SQL query to evaluate the boolean expressions in Expressions table.

Return the result table in any order.

## Tables
```
Variables table:
+------+-------+
| name | value |
+------+-------+
| x    | 66    |
| y    | 77    |
+------+-------+

Expressions table:
+--------------+----------+---------------+
| left_operand | operator | right_operand |
+--------------+----------+---------------+
| x            | >        | y             |
| x            | <        | y             |
| x            | =        | y             |
| y            | >        | x             |
| y            | <        | x             |
| x            | =        | x             |
+--------------+----------+---------------+

Result table:
+--------------+----------+---------------+-------+
| left_operand | operator | right_operand | value |
+--------------+----------+---------------+-------+
| x            | >        | y             | false |
| x            | <        | y             | true  |
| x            | =        | y             | false |
| y            | >        | x             | true  |
| y            | <        | x             | false |
| x            | =        | x             | true  |
+--------------+----------+---------------+-------+
As shown, you need find the value of each boolean exprssion in the table using the variables table.
```

## Code
```
    SELECT left_operand, operator, right_operand, 
            (CASE operator  WHEN '>' THEN IF(v1.value > v2.value, 'true', 'false')
                            WHEN '<' THEN IF(v1.value < v2.value, 'true', 'false')
                            WHEN '=' THEN IF(v1.value = v2.value, 'true', 'false')
                            END) AS value

    FROM Expressions e LEFT JOIN Variables v1 ON e.left_operand = v1.name
                       LEFT JOIN Variables v2 ON e.right_operand = v2.name
```

## Summary
CASE WHEN has a great structure, so it's easier to read and understand than IF when there are several conditions.<br/>
IF is great when there are only two situations.

Use CASE WHEN and IF together for complicated situation, so people can read easily

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
