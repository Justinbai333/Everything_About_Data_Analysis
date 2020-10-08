[原题链接](https://leetcode-cn.com/problems/replace-employee-id-with-the-unique-identifier/)

SQL Code：

```sql
# Write your MySQL query statement below

SELECT unique_id, name
FROM Employees left join EmployeeUNI
ON Employees.id = EmployeeUNI.id
```
