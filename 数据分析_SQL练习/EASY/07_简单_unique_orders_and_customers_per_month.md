[原题链接](https://leetcode-cn.com/problems/unique-orders-and-customers-per-month/)

SQL Code:

```sql
# Write your MySQL query statement below

SELECT
date_format(order_date, '%Y-%m') as month,
COUNT(DISTINCT customer_id) as customer_count,
COUNT(order_id) as order_count
FROM Orders
where invoice > 20
group by date_format(order_date, '%Y-%m')
```

**Note:**

- date_format的使用，可以对日期的格式进行变化，配合group by可以按照月份统计数据
- COUNT()的使用，此处不需要用到窗口函数
