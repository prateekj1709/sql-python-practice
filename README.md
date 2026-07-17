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
-----------------------------------------------------------------------------------------------------------------------------------------
Question link
https://platform.stratascratch.com/coding/10090-find-the-percentage-of-shipable-orders/official-solution?code_type=3

Solution
select 100*sum(case when c.address is not null then 1 else 0 end) / count(*) as percent_shipable
from orders o
join customers c on c.id = o.cust_id

-----------------------------------------------------------------------------------------------------------------------------------------
Question link
https://platform.stratascratch.com/coding/10085-facebook-matching-users-pairs?code_type=3

Solution
select f1.id, f2.id
from facebook_employees f1
join facebook_employees f2 on f1.location = f2.location and f1.age != f2.age and f1.gender = f2.gender and f1.is_senior != f2.is_senior

-----------------------------------------------------------------------------------------------------------------------------------------
Question link
https://platform.stratascratch.com/coding/10078-find-matching-hosts-and-guests-in-a-way-that-they-are-both-of-the-same-gender-and-nationality?code_type=3

Solution
select distinct h.host_id, g.guest_id
from airbnb_hosts h
join airbnb_guests g on h.gender = g.gender and h.nationality= g.nationality
order by h.host_id

-----------------------------------------------------------------------------------------------------------------------------------------
Question link 
https://platform.stratascratch.com/coding/10077-income-by-title-and-gender?code_type=3

Solution
with cte as (select worker_ref_id, sum(bonus) as total_bonus
from sf_bonus
group by worker_ref_id)

select e.employee_title, e.sex, avg(salary+total_bonus) as avg_comp
from cte c 
join sf_employee e on e.id = c.worker_ref_id
group by e.employee_title, e.sex

-----------------------------------------------------------------------------------------------------------------------------------------
Question link
https://platform.stratascratch.com/coding/10049-reviews-of-categories?code_type=3

Soln)
with recursive cte as (select business_id, review_count, 
case when position(';' in categories) > 0
then left(categories, position(';' in categories)-1)
else categories
end as category,
case when position(';' in categories) > 0
then substring(categories from position(';' in categories)+1)
else null
end as remaining
from yelp_business

union all 

Select business_id, review_count, 
case when position(';' in remaining) > 0
then left(remaining, position(';' in remaining)-1)
else remaining
end as category,
case when position(';' in remaining) > 0
then substring(remaining from position(';' in remaining)+1)
else null
end as remaining
from cte
where remaining is not null
)

select category, sum(review_count) as counts
from cte 
group by category
order by counts desc

-----------------------------------------------------------------------------------------------------------------------------------------
Question link 
https://platform.stratascratch.com/coding/10025-find-all-possible-varieties-which-occur-in-either-of-the-winemag-datasets/official-solution?code_type=3

Soln)
with cte as (select variety
from winemag_p1
group by variety

union 

select variety
from winemag_p2
group by variety)

select *
from cte
order by variety asc

-----------------------------------------------------------------------------------------------------------------------------------------
Question link




