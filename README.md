# SQL Case Study 3 : Foodie-Fi

https://8weeksqlchallenge.com/case-study-3/

## A. Customer Journey

**Question 1:** Based off the 8 sample customers provided in the sample from the subscriptions table, write a brief description about each customer’s onboarding journey.
Try to keep it as short as possible - you may also want to run some sort of join to make your explanations a bit easier!

---

## SQL Code

```sql
SELECT customer_id,s.plan_id,plan_name,start_date,price FROM subscriptions s 
LEFT JOIN plans p on 
s.plan_id = p.plan_id
WHERE customer_id IN  (1,2,11,13,15,16,18,19)
ORDER BY customer_id,plan_id
```

| customer_id | plan_id | plan_name     | start_date | price  |
| ----------- | ------- | ------------- | ---------- | ------ |
| 1           | 0       | trial         | 2020-08-01 | 0.00   |
| 1           | 1       | basic monthly | 2020-08-08 | 9.90   |
| 2           | 0       | trial         | 2020-09-20 | 0.00   |
| 2           | 3       | pro annual    | 2020-09-27 | 199.00 |
| 11          | 0       | trial         | 2020-11-19 | 0.00   |
| 11          | 4       | churn         | 2020-11-26 |        |
| 13          | 0       | trial         | 2020-12-15 | 0.00   |
| 13          | 1       | basic monthly | 2020-12-22 | 9.90   |
| 13          | 2       | pro monthly   | 2021-03-29 | 19.90  |
| 15          | 0       | trial         | 2020-03-17 | 0.00   |
| 15          | 2       | pro monthly   | 2020-03-24 | 19.90  |
| 15          | 4       | churn         | 2020-04-29 |        |
| 16          | 0       | trial         | 2020-05-31 | 0.00   |
| 16          | 1       | basic monthly | 2020-06-07 | 9.90   |
| 16          | 3       | pro annual    | 2020-10-21 | 199.00 |
| 18          | 0       | trial         | 2020-07-06 | 0.00   |
| 18          | 2       | pro monthly   | 2020-07-13 | 19.90  |
| 19          | 0       | trial         | 2020-06-22 | 0.00   |
| 19          | 2       | pro monthly   | 2020-06-29 | 19.90  |
| 19          | 3       | pro annual    | 2020-08-29 | 199.00 |

---
Explanation : 

