# Leetcode

Several friends at a cinema ticket office would like to reserve consecutive available seats.
Can you help to query all the consecutive available seats order by the seat_id using the following cinema table?
| seat_id | free |
|---------|------|
| 1       | 1    |
| 2       | 0    |
| 3       | 1    |
| 4       | 1    |
| 5       | 1    |
 

Your query should return the following result for the sample case above.
 

| seat_id |
|---------|
| 3       |
| 4       |
| 5       |
Note:
The seat_id is an auto increment int, and free is bool ('1' means free, and '0' means occupied.).
Consecutive available seats are more than 2(inclusive) seats consecutively available.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/consecutive-available-seats

------ self join 兩個一樣的表格 a, b ------
1. 創 cinema table a, b
2. 當 a 的 seat_id 後面的位子是 b 或是 b後面是a 
3. a 是free, b也是free 就是連續2
4, join後 是 order by a 的id

# Write your MySQL query statement below
select distinct a.seat_id from cinema a ,cinema b
where (a.seat_id+1=b.seat_id or b.seat_id+1=a.seat_id)
and a.free=1
and b.free=1
order by a.seat_id

------ LAG 兩個一樣的表格 a, b ------

SELECT SEAT_ID
FROM (SELECT *, LAG(FREE, 1) OVER(ORDER BY SEAT_ID) AS L1, LEAD(FREE, 1) OVER(ORDER BY SEAT_ID) AS L2
      FROM CINEMA) AS A
WHERE FREE = 1
AND (L1 = 1 OR L2 = 1)
ORDER BY 1;

------  LAG   & LEAD 用法 ------
LAG(輸出值 以＿＿為計算的 ,1) --> default 可以不寫1
LAG(＿＿＿,1) OVER (PARTITION BY 同一組客戶   
                    ORDER BY 日期, 座位 ) AS 前一格PreviousValue（自己命名）
 
LEAD (輸出值 以＿＿為計算的 ,1) --> default 
LEAD(＿＿＿,1) OVER (PARTITION BY 同一組客戶   
                    ORDER BY 日期, 座位 ) AS 後一格 NextValue（自己命名）
   
                    
                    
                    
                    
