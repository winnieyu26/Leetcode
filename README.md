# Write your MySQL query statement below

select employee_id, 
case when employee_id %2 =1 AND name like 'M%'
    then 0 else salary 
    end as bonus 
from Employees

-- name does not start with the character 'M': 
    1. substr(name, 1, 1) != 'M'    --> substr(str,start,length)
    2. name NOT LIKE 'M%'    --> M開頭 ％後面可以是num or char 

--  odd  除以2 餘1
--    1.  MOD（a,b): a/b的餘數 MOD（a,b）= 1 ex: MOD(id,2) != 0 或是  MOD(id,2) = 1
--    2.  odd:  id % 2 != 1 
--    3.  odd: id&1==1  ex: 奇数 employee_id& 1

--  even  除以2 餘0 
--    1.  MOD（a,b）表示a/b餘數 MOD（a,b）= 0 
--    2.  odd:  id % 2 = 0 
--    3.  odd: id&1== 0

-- 條件 case when 
case when (A AND B +not like  
    then 結果 
    else 另一個結果 end) as “col名”

 select employee_id,
    (CASE WHEN MOD(employee_id,2) != 0 AND name NOT LIKE 'M%' THEN salary ELSE 0 END) AS bonus
    FROM Employees


SELECT employee_id
	, CASE 
		WHEN employee_id % 2 != 1
		OR substr(name, 1, 1) = 'M' THEN 0
		ELSE salary
	END AS bonus
FROM employees

-- employee_id&1==1,为奇数
-- employee_id&1==0,为偶数
-- 奇数: employee_id& 1

-- 名字不能以m开头,因此name not like 'M%',其中%代表匹配任何字符串的零个或多个字符。


# Write your MySQL query statement below
-- case when 條件(not like) then __  else __ end 
select employee_id,
    case 
        WHEN employee_id&1 and  name not like 'M%' then salary 
        else 0
    end as 'bonus' 
from Employees;

-- 利用substr函数可以进一步降低运行时间
substr(str,start,length), 原始字符串,起始位置(注意从1开始计算),取字符长度
substr(name, 1, 1)代表 name的第一个字符开始,取1个字符,判断!='M'

# case when  條件 then __  else __ end 
select employee_id,
    case WHEN employee_id&1 and  substr(name, 1, 1) != 'M' then salary 
        else 0
    end as bonus 
from Employees;

