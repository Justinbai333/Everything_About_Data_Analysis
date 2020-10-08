[原题链接](https://leetcode-cn.com/problems/product-sales-analysis-iii/)

SQL Code:

**Solution 1**

```sql
# Write your MySQL query statement below

SELECT product_id, year AS first_year, quantity, price
FROM (
    SELECT *, RANK() OVER(PARTITION BY product_id ORDER BY year) AS year_rnk
    FROM Sales
) AS temp
WHERE year_rnk = 1
```
