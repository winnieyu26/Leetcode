# Amazon interview question using a Window function --> partition by
Q: percentage of total spent

STEPS: 
-- 1. join the orders with customers using an inner join
 
 #select * (可以先看全部的table 長什麼樣）
  select first_name, order_details, order_cost
  from orders o
  join customers c
  on c.id = o.cust_id  # --> join on 一樣的值
  order by first_name  #--> 
  
-- 2. sum total sepnd (only by each customer)  --> use WINDOW FUNCTION 

改order_cost 因為要算：
  sum(order_cost) over(partition by first_name) as total_spent
  
-- 3. find % total spent --> order_cost/ sum(order_cost)) by customer 
  
  sum(order_cost) over(partition by first_name) --> 出來為"0" 因為是integer, 
  sum(order_cost) over(partition by first_name)::FLOAT  -->改成float, 好多小數點 
  
-- 4. round (*100)

  round(order_cost/ sum(order_cost) over(partition by first_name)::FLOAT *100) --> round 取兩位
  round(order_cost/ sum(order_cost) over(partition by first_name)::FLOAT *100) as percentage_of_total_spend  --> 命名

------ ALL TOGEHER ------

  select first_name, 
         order_details,
          round(order_cost/ sum(order_cost) over(partition by first_name)::FLOAT *100) as percentage_of_total_spend
  from orders o
  join customers c
  on c.id = o.cust_id  
  order by first_name  
  
  
