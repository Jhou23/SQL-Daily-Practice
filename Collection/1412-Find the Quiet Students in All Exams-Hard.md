## Problem Introduction
A "quite" student is the one who took at least one exam and didn't score neither the high score nor the low score.

Write an SQL query to report the students (student_id, student_name) being "quiet" in ALL exams.

Don't return the student who has never taken any exam. Return the result table ordered by student_id.

## Tables
```
Student table:
+-------------+---------------+
| student_id  | student_name  |
+-------------+---------------+
| 1           | Daniel        |
| 2           | Jade          |
| 3           | Stella        |
| 4           | Jonathan      |
| 5           | Will          |
+-------------+---------------+

Exam table:
+------------+--------------+-----------+
| exam_id    | student_id   | score     |
+------------+--------------+-----------+
| 10         |     1        |    70     |
| 10         |     2        |    80     |
| 10         |     3        |    90     |
| 20         |     1        |    80     |
| 30         |     1        |    70     |
| 30         |     3        |    80     |
| 30         |     4        |    90     |
| 40         |     1        |    60     |
| 40         |     2        |    70     |
| 40         |     4        |    80     |
+------------+--------------+-----------+

Result table:
+-------------+---------------+
| student_id  | student_name  |
+-------------+---------------+
| 2           | Jade          |
+-------------+---------------+

For exam 1: Student 1 and 3 hold the lowest and high score respectively.
For exam 2: Student 1 hold both highest and lowest score.
For exam 3 and 4: Studnet 1 and 4 hold the lowest and high score respectively.
Student 2 and 5 have never got the highest or lowest in any of the exam.
Since student 5 is not taking any exam, he is excluded from the result.
So, we only return the information of Student 2.
```

## Code
```
    WITH exam_rank AS (
        SELECT student_id,
        IF(DENSE_RANK() OVER(PARTITION BY exam_id ORDER BY score DESC)=1,1,0) desc_rank,
        IF(DENSE_RANK() OVER(PARTITION BY exam_id ORDER BY score )=1,1,0) asc_rank
        FROM Exam
        ),

    quiet_student_id AS (
        SELECT student_id
        FROM exam_rank
        GROUP BY student_id
        HAVING SUM(desc_rank) = 0 AND SUM(asc_rank) = 0
        )

    SELECT q.student_id,student_name
    FROM quiet_student_id q LEFT JOIN Student s ON q.student_id =s.student_id
    ORDER BY q.student_id
```

## Summary
```
Method
--
  # To get the quiet student(took >1 exams and didn't score neither high nor low)
  # 1. get the "dense rank" for each exam, since students may score the same high score
  # 2. mark the high and low scores as 1 and others as 0 for further aggregation
  # 3. Obtain the quiet students by sum up their rank, 0 means quiet
  # more than 1 means that student have been noticed for high and
  # 4. Join student table with quiet_student_id table to get name
  # and filter out who haven't taken any exam before.

Others:
1.mark different status with signals such as integer and aggregate can solve complicated questions with less code.
2. combine IF/CASE WHEN with WINDOW function
IF ((DENSE_RANK() ....) = 1, 1, 0) to avoid more CTE or steps.
```

## Feedback:
If you have any suggestions, I would like to hear from you:<br/>
**Email**: jinxin.hou1994@gmail.com<br/>
**LinkedIn**: http://www.linkedin.com/in/jinxin-hou-a2708898
