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
### Explanation 

**Customer ID 1:** Initially started with a 7-day free trial and downgraded to Basic Monthly after the free trial.

**Customer ID 2:** Initially started with a 7-day free trial and upgraded to the Pro Annual plan after the trial period.

**Customer ID 11:** Initially started with a 7-day free trial and canceled the plan after the 7-day trial.

**Customer ID 13:** Initially started with a 7-day free trial, then downgraded to the Basic Monthly plan for a week, then upgraded to Pro Monthly.

**Customer ID 15:** Initially started with a 7-day free trial, and the plan automatically upgraded to Pro Monthly as the customer did not cancel, downgrade, or upgrade; five days later, the customer canceled the subscription.

**Customer ID 16:** Initially started with a 7-day free trial, then downgraded to the Basic Monthly plan, stayed on the same plan for 4 months, then upgraded to the Pro Annual plan.

**Customer ID 18:** Initially started with a 7-day free trial, and the plan automatically upgraded to Pro Monthly as the customer did not cancel, downgrade, or upgrade.

**Customer ID 19:** Initially started with a 7-day free trial, and the plan automatically upgraded to Pro Monthly as the customer did not cancel, downgrade, or upgrade, stayed on the same plan for 2 months before upgrading to the Pro Annual plan.


---

## B. Data Analysis Questions 


**Question 1:** How many customers has Foodie-Fi ever had?

**Question 2 :** What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value

**Question 3 :** What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name

**Question 4 :** What is the customer count and percentage of customers who have churned rounded to 1 decimal place?

**Question 5 :** How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?

**Question 6 :** What is the number and percentage of customer plans after their initial free trial?

**Question 7 :** What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?

**Question 8 :** How many customers have upgraded to an annual plan in 2020?

**Question 9 :** How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?

**Question 10 :** Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)

**Question 11 :** How many customers downgraded from a pro monthly to a basic monthly plan in 2020?


