# sql-python-practice
SQL exercises Window functions Joins Python practice Small experiments Learning notes

Question link 
https://platform.stratascratch.com/coding/10087-find-all-posts-which-were-reacted-to-with-a-heart?code_type=3

Solution
select distinct fb.*
from facebook_posts as fb
join (select * from facebook_reactions
where reaction = 'heart') as fr on fb.post_id = fr.post_id

--------------------------------------------------------------------------------------------------------------------------------------
Question link 
https://platform.stratascratch.com/coding/2054-consecutive-days?code_type=3

Solution
with cte1 as (select distinct user_id, record_date
from sf_events)
,
cte as (select record_date, user_id, record_date - row_number() over(partition by user_id) as grp 
from cte1)

select user_id
from cte 
group by user_id, grp
having count(*) >= 3

--------------------------------------------------------------------------------------------------------------------------------------
Question link 
https://platform.stratascratch.com/coding/10172-best-selling-item?code_type=3

Solution 
with cte as (select month(invoiceDate) as month, description, sum(unitprice * quantity) as sum_amt
from online_retail
where invoiceno not like 'c%'
group by month(invoiceDate), description)
,
cte2 as (Select month, description, sum_amt, row_number() over(partition by month order by sum_amt desc) as rnk
from cte)

select month, description, sum_amt
from cte2
where rnk =1 

---------------------------------------------------------------------------------------------------------------------------------------
Question link
https://platform.stratascratch.com/coding/10127-calculate-samanthas-and-lisas-total-sales-revenue?code_type=3

Solution
select sum(sales_revenue) as total_sales_revenue
from sales_performance
where salesperson in ('Samantha', 'Lisa')

---------------------------------------------------------------------------------------------------------------------------------------
Question link
https://platform.stratascratch.com/coding/10130-find-the-number-of-inspections-for-each-risk-category-by-inspection-type?code_type=3

Solution
WITH cte AS (
    SELECT
        inspection_type,
        risk_category,
        COUNT(*) AS total_no_risk
    FROM sf_restaurant_health_violations
    GROUP BY inspection_type, risk_category
    ORDER BY inspection_type
)

SELECT
    inspection_type,
    SUM(CASE WHEN risk_category = 'High Risk' THEN total_no_risk ELSE 0 END) AS high_risk,
    SUM(CASE WHEN risk_category = 'Low Risk' THEN total_no_risk ELSE 0 END) AS low_risk,
    SUM(CASE WHEN risk_category = 'Moderate Risk' THEN total_no_risk ELSE 0 END) AS moderate_risk,
    SUM(CASE WHEN risk_category IS NULL THEN total_no_risk ELSE 0 END) AS no_risk,
    SUM(total_no_risk) AS total_risk
FROM cte
GROUP BY inspection_type
----------------------------------------------------------------------------------------------------------------------------------------
Question link

