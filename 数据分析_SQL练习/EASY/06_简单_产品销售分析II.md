[原题链接](https://leetcode-cn.com/problems/product-sales-analysis-ii)

SQL Code:

```sql
# Write your MySQL query statement below

SELECT DISTINCT s.product_id as product_id, sum(quantity) over (partition by s.product_id) as total_quantity
FROM Sales s left join Product p
ON s.product_id = p.product_id
```

**Note:** 在第一句SELECT中，product_id因为在两表同时出现所以会Ambiguous，需要具体说明是s.product_id
