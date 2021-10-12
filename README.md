Write an SQL query to report the IDs of all suspicious bank accounts.

A bank account is suspicious if the total income exceeds the max_income for this account for two or more consecutive months. The total income of an account in some month is the sum of all its deposits in that month (i.e., transactions of the type 'Creditor').

Return the result table in ascending order by transaction_id.

The query result format is in the following example:

 

Accounts table:
+------------+------------+
| account_id | max_income |
+------------+------------+
| 3          | 21000      |
| 4          | 10400      |
+------------+------------+

Transactions table:
+----------------+------------+----------+--------+---------------------+
| transaction_id | account_id | type     | amount | day                 |
+----------------+------------+----------+--------+---------------------+
| 2              | 3          | Creditor | 107100 | 2021-06-02 11:38:14 |
| 4              | 4          | Creditor | 10400  | 2021-06-20 12:39:18 |
| 11             | 4          | Debtor   | 58800  | 2021-07-23 12:41:55 |
| 1              | 4          | Creditor | 49300  | 2021-05-03 16:11:04 |
| 15             | 3          | Debtor   | 75500  | 2021-05-23 14:40:20 |
| 10             | 3          | Creditor | 102100 | 2021-06-15 10:37:16 |
| 14             | 4          | Creditor | 56300  | 2021-07-21 12:12:25 |
| 19             | 4          | Debtor   | 101100 | 2021-05-09 15:21:49 |
| 8              | 3          | Creditor | 64900  | 2021-07-26 15:09:56 |
| 7              | 3          | Creditor | 90900  | 2021-06-14 11:23:07 |
+----------------+------------+----------+--------+---------------------+

Result table:
+------------+
| account_id |
+------------+
| 3          |
+------------+

For account 3:
- In 6-2021, the user had an income of 107100 + 102100 + 90900 = 300100.
- In 7-2021, the user had an income of 64900.
We can see that the income exceeded the max income of 21000 for two consecutive months, so we include 3 in the result table.

For account 4:
- In 5-2021, the user had an income of 49300.
- In 6-2021, the user had an income of 10400.
- In 7-2021, the user had an income of 56300.
We can see that the income exceeded the max income in May and July, but not in June. Since the account did not exceed the max income for two consecutive months, we do not include it in the result table.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/suspicious-bank-accounts


+----------------+------------+----------+--------+---------------------+


# Write your MySQL query statement below

# 1. 算 total income 
With t as 
    (SELECT account_id, SUM(amount) as total_income
    , EXTRACT(YEAR_MONTH FROM day) as 'month'   
    # EXTRACT出來:把年月挑出來 可以相減
    # 不能用left() 因為是string 無法相減  
    FROM Transactions 
    WHERE type = 'Creditor' 
    GROUP BY account_id, LEFT(day, 7)) 
                        #LEFT() 是string

# 2. consecutive month 
select DISTINCT t1.account_id
FROM t t1  # 用上面的表
LEFT JOIN t t2  #右邊的表month大
ON t2.month - t1.month = 1 
AND t2.account_id = t1.account_id

# 3. 對比 total_income 跟 max 
LEFT JOIN Accounts a ON 
t1.account_id = a.account_id 
WHERE t1.total_income > max_income 
AND t2.total_income > max_income 
# 第１個月比max大
# 第2個月也比max大
    
    
# 法2

# Write your MySQL query statement below

WITH t AS (
SELECT t.account_id, DATE(day) 'day'
FROM Transactions t LEFT JOIN Accounts a 
ON t.account_id = a.account_id 
WHERE type = 'Creditor'
GROUP BY account_id, LEFT(day, 7)
HAVING SUM(amount) > AVG(max_income)
)

SELECT DISTINCT t1.account_id
FROM t t1, t t2 
WHERE t1.account_id = t2.account_id 
AND PERIOD_DIFF(MONTH(t1.day),MONTH(t2.day)) = 1
    
  
