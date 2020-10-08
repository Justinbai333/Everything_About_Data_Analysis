[原题链接](https://leetcode-cn.com/problems/project-employees-iii/)

SQL Code:

**Solution 1**

```sql
# Write your MySQL query statement below

SELECT project_id, employee_id
FROM (
    SELECT project_id, p.employee_id, rank() OVER(PARTITION BY project_id ORDER BY experience_years DESC)       AS experience
    FROM Project p LEFT JOIN Employee e
    ON p.employee_id = e.employee_id
)  AS temp
WHERE experience = 1
```
