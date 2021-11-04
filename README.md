Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

Return the result table in any order.

The query result format is in the following example.

 

Example 1:

Input: 
Transactions table:
+------+---------+----------+--------+------------+
| id   | country | state    | amount | trans_date |
+------+---------+----------+--------+------------+
| 121  | US      | approved | 1000   | 2018-12-18 |
| 122  | US      | declined | 2000   | 2018-12-19 |
| 123  | US      | approved | 2000   | 2019-01-01 |
| 124  | DE      | approved | 2000   | 2019-01-07 |
+------+---------+----------+--------+------------+
Output: 
+----------+---------+-------------+----------------+--------------------+-----------------------+
| month    | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
+----------+---------+-------------+----------------+--------------------+-----------------------+
| 2018-12  | US      | 2           | 1              | 3000               | 1000                  |
| 2019-01  | US      | 1           | 1              | 2000               | 2000                  |
| 2019-01  | DE      | 1           | 1              | 2000               | 2000                  |
+----------+---------+-------------+----------------+--------------------+-----------------------+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/monthly-transactions-i
+----------+---------+-------------+----------------+--------------+----------+---------+-------------+----------------+--------------

# Write your MySQL query statement below


select Left(trans_date,7) as month,
country,
count(trans_date) trans_count, 
SUM(IF(state = 'approved',1,0)) approved_count,   ***這行 要算approve 記得用ＳＵＭ（ＩＦ（
SUM(amount) trans_total_amount,
SUM(IF(state = 'approved',amount, 0)) approved_total_amount  ***這行 要算approve 記得用ＳＵＭ（ＩＦ（
FROM Transactions
GROUP BY month, country

