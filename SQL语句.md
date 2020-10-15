# SQL语句

## easy

#### 1. [176. 第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary/)

```mysql
# Write your MySQL query statement below
select IFNULL (
    (select distinct Salary from Employee 
    order by Salary desc limit 1,1),
    null
) as 'SecondHighestSalary'
```

