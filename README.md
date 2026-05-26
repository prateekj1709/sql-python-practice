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

--------------------------------------------------------------------------------------------------------------------------------------
