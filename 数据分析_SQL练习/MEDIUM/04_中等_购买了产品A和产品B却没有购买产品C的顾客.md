[原题链接](https://leetcode-cn.com/problems/customers-who-bought-products-a-and-b-but-not-c/)

SQL Code:

**Solution 1**

```sql
# Write your MySQL query statement below

SELECT customer_id, customer_name
FROM (
    SELECT o.customer_id, customer_name, SUM(CASE
        WHEN product_name = 'A' THEN 1  
        WHEN product_name = 'B' THEN 1
        WHEN product_name = 'C' THEN -1
        ELSE 0 END) AS score
    FROM Orders o LEFT JOIN Customers c
    ON o.customer_id = c.customer_id
    GROUP BY o.customer_id
) AS temp
WHERE score = 2
```

**Solution 2 整理一下**

```sql
# Write your MySQL query statement below

SELECT o.customer_id, customer_name
FROM Orders o LEFT JOIN Customers c
ON o.customer_id = c.customer_id
GROUP BY o.customer_id
HAVING SUM(CASE
    WHEN product_name = 'A' THEN 1  
    WHEN product_name = 'B' THEN 1
    WHEN product_name = 'C' THEN -1
    ELSE 0 END) = 2
```


**Note:**

- GROUP BY只能在有聚合函数的情况下使用，聚合函数可以放在HAVING里不一定非要在SELECT里
