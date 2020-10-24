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

#### medium

#### 1. [184. 部门工资最高的员工](https://leetcode-cn.com/problems/department-highest-salary/)

```mysql
# Write your MySQL query statement below
# 抄来的答案，where部分没有头绪，需要再看看书
SELECT
    Department.NAME AS Department,
    Employee.Name AS Employee,
    Salary
FROM 
    Employee,
    Department
WHERE 
    Employee.DepartmentId = Department.Id
    AND (Employee.DepartmentId,Salary)
    IN(SELECT DepartmentId,max(Salary)
    FROM Employee
    GROUP BY DepartmentId)
```

