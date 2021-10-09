# Leetcode


# Write your MySQL query statement below

# 用 “recursive”
# 用 UNION ALL 填滿所有的數

WITH RECURSIVE t as (
    select 1 as customer_id #不用逗號    # 沒有from 哪個 table
    UNION ALL
    select customer_id+1 from t  # from recursive t table 來的
    where customer_id < 
        (select MAX(customer_id) 
        from Customers))   # customer_id will not exceed max 100       
                                
select customer_id as ids 
from t
WHERE customer_id NOT IN (
    select cusomter_id from Customers) 
