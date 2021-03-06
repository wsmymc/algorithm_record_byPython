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

#### 2. [177. 第N高的薪水](https://leetcode-cn.com/problems/nth-highest-salary/)

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
# 单表查询玩法
BEGIN
    SET N := N-1;
  RETURN (
      # Write your MySQL query statement below.
        select Salary from  Employee
        GROUP BY
                Salary
        ORDER BY
        Salary DESC
        LIMIT N,1
      
  );
END
```

#### 3. [178. 分数排名](https://leetcode-cn.com/problems/rank-scores/)

```mysql

# Write your MySQL query statement below
select Score ,
DENSE_RANK() OVER(ORDER BY Score DESC) AS 'RANK' FROM Scores;
```

#### 4. [180. 连续出现的数字](https://leetcode-cn.com/problems/consecutive-numbers/)

```mysql
# Write your MySQL query statement below
SELECT DISTINCT
    l1.Num AS ConsecutiveNums
FROM
    Logs l1,
    Logs l2,
    Logs l3
WHERE
    l1.Id = l2.Id - 1
    AND l2.Id = l3.Id - 1
    AND l1.Num = l2.Num
    AND l2.Num = l3.Num
```

#### 5. [626. 换座位](https://leetcode-cn.com/problems/exchange-seats/)

```python
# Write your MySQL query statement below
SELECT
    (CASE
        WHEN MOD(id, 2) != 0 AND counts != id THEN id + 1
        WHEN MOD(id, 2) != 0 AND counts = id THEN id
        ELSE id - 1
    END) AS id,
    student
FROM
    seat,
    (SELECT
        COUNT(*) AS counts
    FROM
        seat) AS seat_counts
ORDER BY id ASC;

```

#### 6. [596. 超过5名学生的课](https://leetcode-cn.com/problems/classes-more-than-5-students/)

```sql
# Write your MySQL query statement below
SELECT
    class
FROM
    (SELECT
        class, COUNT(DISTINCT student) AS num
    FROM
        courses
    GROUP BY class) AS temp_table
WHERE
    num >= 5
;



// 或者
# Write your MySQL query statement below
SELECT
    class
FROM
    courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5
;

```

