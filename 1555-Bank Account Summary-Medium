## Problem Introduction
Standard Chartered Bank helps its coders in making virtual payments. Our bank records all transactions in the table Transaction, we want to find out the current balance of all users and check wheter they have breached their credit limit (If their current credit is less than 0).

Write an SQL query to report.

- user_id
- user_name
- credit, current balance after performing transactions.  
- credit_limit_breached, check credit_limit ("Yes" or "No")


## Tables
```
Users table:
+------------+--------------+-------------+
| user_id    | user_name    | credit      |
+------------+--------------+-------------+
| 1          | Moustafa     | 100         |
| 2          | Jonathan     | 200         |
| 3          | Winston      | 10000       |
| 4          | Luis         | 800         | 
+------------+--------------+-------------+

Transactions table:
+------------+------------+------------+----------+---------------+
| trans_id   | paid_by    | paid_to    | amount   | transacted_on |
+------------+------------+------------+----------+---------------+
| 1          | 1          | 3          | 400      | 2020-08-01    |
| 2          | 3          | 2          | 500      | 2020-08-02    |
| 3          | 2          | 1          | 200      | 2020-08-03    |
+------------+------------+------------+----------+---------------+

Result table:
+------------+------------+------------+-----------------------+
| user_id    | user_name  | credit     | credit_limit_breached |
+------------+------------+------------+-----------------------+
| 1          | Moustafa   | -100       | Yes                   | 
| 2          | Jonathan   | 500        | No                    |
| 3          | Winston    | 9900       | No                    |
| 4          | Luis       | 800        | No                    |
+------------+------------+------------+-----------------------+
Moustafa paid $400 on "2020-08-01" and received $200 on "2020-08-03", credit (100 -400 +200) = -$100
Jonathan received $500 on "2020-08-02" and paid $200 on "2020-08-08", credit (200 +500 -200) = $500
Winston received $400 on "2020-08-01" and paid $500 on "2020-08-03", credit (10000 +400 -500) = $9990
Luis didn't received any transfer, credit = $800
```

## Code
```
# calculate current credit of each customer
WITH credit AS (SELECT user_id, user_name,
                # set payment as negative and income as positive, then sum up
                AVG(credit) + IFNULL(SUM(IF(user_id = paid_by, -1, 1)*amount), 0) AS credit 
                FROM users u LEFT JOIN Transactions t 
                             ON user_id = paid_by 
                             OR user_id = paid_to
                GROUP BY user_id, user_name)
# show who breached credit limit
SELECT *, IF(credit>=0, 'No', 'Yes') credit_limit_breached
FROM credit
```

## Summary
left join with OR to get rows with more than 1 conditions
sum up values with IF or Case when to streamline codes
use CTE to make codes easier to read

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
