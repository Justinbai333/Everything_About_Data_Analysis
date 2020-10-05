[原题链接](https://leetcode-cn.com/problems/sales-analysis-iii/)

SQL Code:

**Solution 1**

```sql
# Write your MySQL query statement below

SELECT  DISTINCT p.product_id, product_name
FROM Sales s LEFT JOIN Product p
ON s.product_id = p.product_id
WHERE s.product_id NOT IN (
    SELECT product_id
    FROM Sales
    WHERE sale_date < '2019-01-01' OR sale_date > '2019-03-31'
)
```

**Solution 2**

```sql
select p.product_id, p.product_name
from sales s, product p
where s.product_id = p.product_id
group by s.product_id
having sum(s.sale_date> '2019-03-31')=0 and sum(s.sale_date < '2019-01-01') = 0
```

**Notes:**
- SUM等式True为1，False为0
