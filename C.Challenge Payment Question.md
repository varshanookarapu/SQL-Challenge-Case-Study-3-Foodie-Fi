## C.Challenge Payment Question

The Foodie-Fi team wants you to create a new payments table for the year 2020 that includes amounts paid by each customer in the subscriptions table with the following requirements:

##### monthly payments always occur on the same day of month as the original start_date of any monthly paid plan
##### upgrades from basic to monthly or pro plans are reduced by the current paid amount in that month and start immediately
##### upgrades from pro monthly to pro annual are paid at the end of the current billing period and also starts at the end of the month period
##### once a customer churns they will no longer make payments

---

## SQL Code

```sql
WITH RECURSIVE 

base AS 
(
SELECT customer_id, s.plan_id,plan_name, 
LAG(s.plan_id) OVER(PARTITION BY customer_id ORDER BY start_date ASC ) as previous_plan_id,
LEAD(s.plan_id) OVER(PARTITION BY customer_id ORDER BY start_date ASC ) as next_plan_id,
start_date, 
LAG(start_date)OVER(PARTITION BY customer_id ORDER BY start_date ASC ) as previous_plan_date,
LEAD(start_date)OVER(PARTITION BY customer_id ORDER BY start_date ASC ) as next_plan_date,price,
LAG(price) OVER(PARTITION BY customer_id ORDER BY start_date ASC ) as previous_price
FROM subscriptions s LEFT JOIN plans p ON 
s.plan_id = p.plan_id

),  

-- WE ARE FINDING THE PERIOD END DATE HERE 
base2 AS 
(
SELECT * , 
(CASE WHEN next_plan_date IS NOT NULL THEN next_plan_date - (INTERVAL '1 DAY')
ELSE DATE '2020-12-31' 
END) :: DATE
AS 
period_end_date 
FROM base
),

-- payments table to have customer_id,plan_id,plan_name,payment_date,amount , payment_order
monthly_payments AS 
(
    -- anchor
    SELECT customer_id, plan_id, plan_name, 
  
    CASE WHEN plan_id = 2 AND previous_plan_id=1 THEN  (start_date + INTERVAL '1 Month') :: DATE 
    ELSE start_date 
    END
    AS payment_date, price AS amount, period_end_date
    FROM base2
    WHERE plan_id IN (1,2) 

    UNION ALL

    -- recursive
    SELECT customer_id, plan_id, plan_name, (payment_date + INTERVAL '1 MONTH')::date AS payment_date, amount, period_end_date
    FROM monthly_payments
    WHERE (payment_date + INTERVAL '1 MONTH') <= LEAST ( period_end_date , DATE '2020-12-31')
),
-- LEAST uses the earlier date of the two dates. 




--  When updgraded from basic to pro monthly plan , we are ensuring the price is updated
basictopromonthly AS
(
SELECT customer_id,plan_id,plan_name,start_date as payment_date, price - previous_price as amount, period_end_date 
FROM base2 
WHERE plan_id = 2 and previous_plan_id = 1 
),

protoannual AS 
(
SELECT customer_id,plan_id,plan_name,start_date as payment_date, price as amount, period_end_date 
FROM base2 
WHERE plan_id = 3 and previous_plan_id = 2 
 ) ,


all_payments AS 
(
SELECT * FROM monthly_payments 
UNION ALL
SELECT * FROM basictopromonthly
UNION ALL
SELECT * from protoannual
)


SELECT customer_id,plan_id,plan_name,payment_date,amount, period_end_date,
ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY payment_date) AS payment_order
FROM all_payments 
WHERE payment_date <= DATE '2020-12-31'
ORDER BY customer_id,payment_date


```

---
To address this challenge, I’m going to create three CTEs then i will combile all three to generate the payments table. In first step I am creating a base table, which contains all the information about the next plan, including the next plan dates, as well as the previous plans and their dates and previous price details.


```sql
WITH base AS 
(
SELECT customer_id,
s.plan_id,
plan_name,
LAG(s.plan_id) OVER(PARTITION BY customer_id ORDER BY start_date ASC) as previous_plan_id,
LEAD(s.plan_id) OVER(PARTITION BY customer_id ORDER BY start_date ASC) as next_plan_id,
start_date,
LAG(start_date) OVER(PARTITION BY customer_id ORDER BY start_date ASC) as previous_plan_date,  
LEAD(start_date) OVER(PARTITION BY customer_id ORDER BY start_date ASC) as next_plan_date,  
price,
LAG(price) OVER(PARTITION BY customer_id ORDER BY start_date ASC) as previous_price   
FROM subscriptions s LEFT JOIN plans p on
s.plan_id = p.plan_id
  
)

SELECT * FROM base ORDER BY customer_id, start_date
```

<img width="1894" height="830" alt="image" src="https://github.com/user-attachments/assets/8d66017a-18c3-4528-bf01-5ac7740ffdc7" />






