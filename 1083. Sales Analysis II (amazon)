Write an SQL query that reports the buyers who have bought S8 but not iPhone. Note that S8 and iPhone are products present in the Product table.

Return the result table in any order.

The query result format is in the following example.

 

Example 1:

Input: 
Product table:
+------------+--------------+------------+
| product_id | product_name | unit_price |
+------------+--------------+------------+
| 1          | S8           | 1000       |
| 2          | G4           | 800        |
| 3          | iPhone       | 1400       |
+------------+--------------+------------+
Sales table:
+-----------+------------+----------+------------+----------+-------+
| seller_id | product_id | buyer_id | sale_date  | quantity | price |
+-----------+------------+----------+------------+----------+-------+
| 1         | 1          | 1        | 2019-01-21 | 2        | 2000  |
| 1         | 2          | 2        | 2019-02-17 | 1        | 800   |
| 2         | 1          | 3        | 2019-06-02 | 1        | 800   |
| 3         | 3          | 3        | 2019-05-13 | 2        | 2800  |
+-----------+------------+----------+------------+----------+-------+
Output: 
+-------------+
| buyer_id    |
+-------------+
| 1           |
+-------------+
Explanation: The buyer with id 1 bought an S8 but did not buy an iPhone. The buyer with id 3 bought both.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sales-analysis-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
+-----------+------------+----------+------------+----------+-------+

# Write your MySQL query statement below

SELECT buyer_id 
FROM Sales s
LEFT JOIN Product p ON s.product_id = p.product_id
GROUP BY buyer_id
having sum(product_name = 'S8') > 0 and  #買s8
       sum(product_name = 'iPhone') = 0  #買iPhone
