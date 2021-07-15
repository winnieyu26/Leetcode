Write an SQL query that reports the books that have sold less than 10 copies in the last year, excluding books that have been available for less than 1 month from today. Assume today is 2019-06-23.

The query result format is in the following example:

Books table:
+---------+--------------------+----------------+
| book_id | name               | available_from |
+---------+--------------------+----------------+
| 1       | "Kalila And Demna" | 2010-01-01     |
| 2       | "28 Letters"       | 2012-05-12     |
| 3       | "The Hobbit"       | 2019-06-10     |
| 4       | "13 Reasons Why"   | 2019-06-01     |
| 5       | "The Hunger Games" | 2008-09-21     |
+---------+--------------------+----------------+

Orders table:
+----------+---------+----------+---------------+
| order_id | book_id | quantity | dispatch_date |
+----------+---------+----------+---------------+
| 1        | 1       | 2        | 2018-07-26    |
| 2        | 1       | 1        | 2018-11-05    |
| 3        | 3       | 8        | 2019-06-11    |
| 4        | 4       | 6        | 2019-06-05    |
| 5        | 4       | 5        | 2019-06-20    |
| 6        | 5       | 9        | 2009-02-02    |
| 7        | 5       | 8        | 2010-04-13    |
+----------+---------+----------+---------------+

Result table:
+-----------+--------------------+
| book_id   | name               |
+-----------+--------------------+
| 1         | "Kalila And Demna" |
| 2         | "28 Letters"       |
| 5         | "The Hunger Games" |
+-----------+--------------------+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unpopular-books


+-----------+--------------------++-----------+--------------------++-----------+----------
# Write your MySQL query statement below

思考点主要是left join的使用以及on和where的辨析
注意两个条件的区别：
过去一年内订单总量少于10本：因为有的书沒order:left join对应的on去筛选，没有的 --> null，
用 ifnull()把null转换成0

available_from < '2019-05-23'：join之後 再用 where 來找一個月內的，
不符合条件的连null都不应该有！如果先对“一个月”进行删除是无效的。

select b.book_id, name
from books b 
left join orders o
on b.book_id = o.book_id and dispatch_date >= '2018-06-23' -- join （id 跟 一年前的） 
 -- 对于‘去年销量’这个条件 如果有些书没在去年买 那么结果是0  而不是exclude掉
 -- reports the books that have sold in the last year 發貨日期dispatch date
 
where available_from < '2019-05-23' -- 不能放在join 因为 他不符合条件要exclude 但是join那些书会出现 如果ifnull变成0之后 他还是会算在符合条件里面
group by b.book_id
having sum(ifnull(quantity, 0)) < 10  --  less than 10 copies 但可能一年前沒有 就有null, 把他變成0 之後加起來sum


# 用datediff 
select b.book_id, b.name
from books b 
left join orders o 
on b.book_id = o.book_id 
and datediff('2019-06-23', o.dispatch_date) <= 365
where datediff('2019-06-23', b.available_from) > 30
group by b.book_id 
having sum(ifnull(o.quantity, 0)) < 10


Q:两个日期限制何时进行处理的问题，如果我们过早的对“一个月”进行删除是无效的。
我们在统计“一年内”销量的时候因为要考虑0销量而没有出现在2018-6-23之后的产品，也就是null的情况，
想到 out join 来进行缺失产品信息的补足，这个时候就会又把早先的“一个月”条目补充回来。
先按照年销量统计，最后同时对两个条件进行相应的筛选，

select book_id,name from
    (select book_id, sum(quantity) as total 
    from orders
    where dispatch_date >'2018-6-23' 
    group by book_id) 
    as newtable
right join books using(book_id)
where ifnull(total,0)<10 and available_from<'2019-05-23'
;
