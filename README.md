Mary is a teacher in a middle school and she has a table seat storing students' names and their corresponding seat ids.

The column id is continuous increment.

Mary wants to change seats for the adjacent students.

Can you write a SQL query to output the result for Mary?

 

+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
For the sample input, the output is:

+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
Note:

If the number of students is odd, there is no need to change the last one's seat.



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/exchange-seats
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。#

#Write your MySQL query statement below
-- The column id is continuous increment.
-- Mary wants to change seats for the adjacent students.

-- Can you write a SQL query to output the result for Mary
# 法 １：查询id和student

若id是偶数，减1
若id是奇数，加1
主要问题在于当总数为奇数时，最后一个id应保持不变，加1会导致空出一位。
解决此问题并不复杂：我们找到最后一位，让它保持不变就可以了。
select 
    if(id%2=0,
        id-1,
        if(id=(select count(distinct id) from seat),
            id,
            id+1)) 
    as id,student 
from seat 
order by id;

https://github.com/Fanlu91/FanluLeetcode


# 法二：使用位操作和 COALESCE()
-- 使用 (id+1)^1-1 计算交换后每个学生的座位 id。

SELECT id, (id+1)^1-1, student FROM seat;
| id | (id+1)^1-1 | student |
|----|------------|---------|
| 1  | 2          | Abbot   |
| 2  | 1          | Doris   |
| 3  | 4          | Emerson |
| 4  | 3          | Green   |
| 5  | 6          | Jeames  |


然后连接原来的座位表和更新 id 后的座位表。
SELECT *
FROM seat s1
    LEFT JOIN seat s2 ON (s1.id+1)^1-1 = s2.id
ORDER BY s1.id; -- 不是group by

| id | student | id | student |
|----|---------|----|---------|
| 1  | Abbot   | 2  | Doris   |
| 2  | Doris   | 1  | Abbot   |
| 3  | Emerson | 4  | Green   |
| 4  | Green   | 3  | Emerson |
| 5  | Jeames  |    |         |
注：前两列来自s1，后两列来自表 s2。

-- 最后输出 s1.id 和 s2.student。但是 id=5 的学生，s1.student 正确，s2.student 为 NULL。因此使用 COALESCE() 为最后一行记录生成正确的输出。

SELECT
    s1.id, COALESCE(s2.student, s1.student) AS student
FROM
    seat s1
        LEFT JOIN
    seat s2 ON ((s1.id + 1) ^ 1) - 1 = s2.id
ORDER BY s1.id;
