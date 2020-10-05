[原题链接](https://leetcode-cn.com/problems/all-people-report-to-the-given-manager/)

SQL Code:

**Solution 1**

```sql
# Write your MySQL query statement below

SELECT e1.employee_id
FROM Employees e1, Employees e2, Employees e3
WHERE e1.manager_id = e2.employee_id AND e2.manager_id = e3.employee_id
AND e3.manager_id = 1 AND e1.employee_id <> 1
```

**Note:**

- 直连三表
- [用PostgreSQL写循环解决更多层级的情况](\https://leetcode-cn.com/problems/all-people-report-to-the-given-manager/solution/jia-ru-leetcode-ke-yi-shi-yong-postgresql-by-sqrtq/)
