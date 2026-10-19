### 1.[176. 第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary/)

编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。

+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。

+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+

两种答案:

#### 1.IFNULL 

```mysql
SELECT
    IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
        LIMIT 1 OFFSET 1),
    NULL) AS SecondHighestSalary
```

OFFSET指定SELECT语句开始查询的数据偏移量。相当于每页显示几条数据。

```mysql
select * from employee limit 1 offset 2;
```

IFNULL(expr1,expr2) : expr1是一个表达式，当expr1为null时 直接使用expr2的值。

```mysql
select IFNULL((select id from employee where id = 4),null)  as data;
```

通过内层循环来判断是否有数据，来决定是否要进行IFNULL函数判断 如果没有数据的话 直接返回Null。

#### 2.子查询

```mysql
SELECT
    (SELECT DISTINCT
            Salary
        FROM
            Employee
        ORDER BY Salary DESC
        LIMIT 1 OFFSET 1) AS SecondHighestSalary
```

通过内查询 查找出第二高的薪水。

### 2.596超过5名学生的课

有一个courses 表 ，有: student (学生) 和 class (课程)。

请列出所有超过或等于5名学生的课。

例如,表:

+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+
应该输出:

+---------+
| class   |
+---------+
| Math    |
+---------+
Note:
学生在每个课中不应被重复计算。

#### 1.groud by 和 having

```mysql
select class from courses  GROUP BY class having count(DISTINCT student) >=5;
```

思路: 计算超过5名学生的课程，因此需要使用分组groud by 分组后 就可以统计了 但是where是在groud by之前进行行级别的过滤 而having是在组级别上进行过滤，因此就可以确定使用 groud by+having组合来解决 去重的化 我想了很久还是不会，看了下答案 答案上是这么写的 count()函数里进行去重 这里重点标记一下。

#### 2.ground by +子查询

```
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
```

思路:前提是需要进行分组操作，所以select class ,count(distinct student) from courses group by class 可以查到分组之后的信息了 ，将查询的表 当作一个临时表来进行操作。

### 3.182.查找重复的电子邮箱

编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。

示例：

+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
根据以上输入，你的查询应返回以下结果：

+---------+
| Email   |
+---------+
| a@b.com |
+---------+
说明：所有电子邮箱都是小写字母。

#### 1.groud by + having 

```mysql
select email from person GROUP BY email having count(*) > 1;
```

#### 2.groud by + 临时表

```
select Email from
(
  select Email, count(Email) as num
  from Person
  group by Email
) as statistic
where num > 1
;
```

### 4.[595. 大的国家](https://leetcode-cn.com/problems/big-countries/)

这里有张 World 表

+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------+------------+------------+--------------+---------------+
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |
+-----------------+------------+------------+--------------+---------------+
如果一个国家的面积超过300万平方公里，或者人口超过2500万，那么这个国家就是大国家。

编写一个SQL查询，输出表中所有大国家的名称、人口和面积。

例如，根据上表，我们应该输出:

+--------------+-------------+--------------+
| name         | population  | area         |
+--------------+-------------+--------------+
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |
+--------------+-------------+--------------+

#### 1.where+or

```mysql
select name,population,area  from world  where area > 3000000 or population  > 25000000;
```

#### 2.where + union

```
SELECT
    name, population, area
FROM
    world
WHERE
    area > 3000000

UNION

SELECT
    name, population, area
FROM
    world
WHERE
    population > 25000000
;
```

union的用法

> UNION操作符y用于合并两个或多个select语句的结果集。
>
> 注意:UNION内部的每个select语句必须拥有相同数量的列，列也必须拥有相似的数据类型，顺序也必须相同
>
> 默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

### 5.[627. 交换工资](https://leetcode-cn.com/problems/swap-salary/)

给定一个 salary 表，如下所示，有 m = 男性 和 f = 女性 的值。交换所有的 f 和 m 值（例如，将所有 f 值更改为 m，反之亦然）。要求只使用一个更新（Update）语句，并且没有中间的临时表。

注意，您必只能写一个 Update 语句，请不要编写任何 Select 语句。

例如：

| id   | name | sex  | salary |
| ---- | ---- | ---- | ------ |
| 1    | A    | m    | 2500   |
| 2    | B    | f    | 1500   |
| 3    | C    | m    | 5500   |
| 4    | D    | f    | 500    |
运行你所编写的更新语句之后，将会得到以下表:

| id   | name | sex  | salary |
| ---- | ---- | ---- | ------ |
| 1    | A    | f    | 2500   |
| 2    | B    | m    | 1500   |
| 3    | C    | f    | 5500   |
| 4    | D    | m    | 500    |

```mysql
UPDATE salary
SET
    sex = CASE sex
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END;
```

### 6.620 有趣的电影

某城市开了一家新的电影院，吸引了很多人过来看电影。该电影院特别注意用户体验，专门有个 LED显示板做电影推荐，上面公布着影评和相关电影描述。

作为该电影院的信息部主管，您需要编写一个 SQL查询，找出所有影片描述为非 boring (不无聊) 的并且 id 为奇数 的影片，结果请按等级 rating 排列。

 

例如，下表 cinema:

+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   1     | War       |   great 3D   |   8.9     |
|   2     | Science   |   fiction    |   8.5     |
|   3     | irish     |   boring     |   6.2     |
|   4     | Ice song  |   Fantacy    |   8.6     |
|   5     | House card|   Interesting|   9.1     |
+---------+-----------+--------------+-----------+
对于上面的例子，则正确的输出是为：

+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   5     | House card|   Interesting|   9.1     |
|   1     | War       |   great 3D   |   8.9     |
+---------+-----------+--------------+-----------+

#### 1.where 

```mysql
select id,movie,description,rating from cinema where mod(id,2) =1 and description != "boring"  order by rating desc ;
```

取余是用函数mod(number1,number2);其返回的是其余数值

mod(id,2) = 1;



### 7.175.组合两个表

表1: Person

+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
表2: Address

+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键


编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：

####  1.左查询

FirstName, LastName, City, State

```
select p.FirstName, p.LastName, a.City, a.State from Person p left join Address a  on p.personId = a.personId;
```

使用左查询 就可以了。

### 8.[181. 超过经理收入的员工](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

Employee 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
给定 Employee 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

+----------+
| Employee |
+----------+
| Joe      |
+----------+

#### 1.使用where +  内连接

```
select * from employee as a,employee b;//笛卡尔积

select * from employee as a,employee b where a.managerId =  b.id and a.salary > b.salary;//筛选条件

select a.name as Employee from employee as a,employee b where a.managerId =  b.id and a.salary > b.salary;
```

#### 2.join on + 内连接

```
select  a.name as Employee from employee as a join employee b on a.managerId = b.id  and a.salary > b.salary;
```

### 9.[183. 从不订购的客户](https://leetcode-cn.com/problems/customers-who-never-order/)

某网站包含两个表，Customers 表和 Orders 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

Customers 表：

+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
Orders 表：

+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
例如给定上述表格，你的查询应返回：

+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+

#### 1.子查询 + not in

```
select Name  as Customers  from Customers where id not in (select distinct(CustomerId) from orders);
```

### 10.[196. 删除重复的电子邮箱](https://leetcode-cn.com/problems/delete-duplicate-emails/)

编写一个 SQL 查询，来删除 Person 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。

+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id 是这个表的主键。
例如，在运行你的查询语句之后，上面的 Person 表应返回以下几行:

+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+

```
delete p1.*
FROM Person p1,
​    Person p2
WHERE
​    p1.Email = p2.Email AND p1.Id > p2.Id
;
```

