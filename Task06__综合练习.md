# Task 06：综合练习

## 练习一：各部门工资最高的员工（难度：中等）

创建`Employee`表，包含所有员工信息，每个员工有其对应的 Id, Name, Salary 和 DepartmentId。

数据库、表的创建及使用

```sql
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

创建`Department`表，包含公司所有部门的信息。

```sql
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。

SQL基本语句

```sql
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

```sql
CREATE DATABASE employee_info;
USE employee_info;
-- 创建Employee表
CREATE TABLE Employee(
Id INTEGER NOT NULL,
Name VARCHAR(20) NOT NULL,
Salary INTEGER NOT NULL,
DepartmentId INTEGER NOT NULL,
PRIMARY KEY (Id)
);
-- 向Employee表写入数据
INSERT INTO Employee VALUES(1, 'Joe', 70000, 1);
INSERT INTO Employee VALUES(2, 'Henry', 80000, 2);
INSERT INTO Employee VALUES(3, 'Sam', 60000, 2);
INSERT INTO Employee VALUES(4, 'Max', 90000, 1);

-- 创建Department表
CREATE TABLE Department(
Id INTEGER NOT NULL,
Name VARCHAR(10) NOT NULL,
PRIMARY KEY (Id)
);
-- 向Department表写入数据
INSERT INTO Department VALUES(1, 'IT');
INSERT INTO Department VALUES(2, 'Sales');

-- Query查询
-- 首先将两表进行内连结
-- 使用(DepartmentId,MAX(Salary))合并子查询建立->部门+最高薪酬,然后使用IN筛选
-- IN谓语 可以轻松应对多个最高工资的情况，通用而实在，推荐。
SELECT D.Name AS 'Department', E.Name AS 'Employee', E.Salary
FROM Employee AS E INNER JOIN Department AS D ON E.DepartmentId = D.Id
WHERE (E.DepartmentId,E.Salary) IN (SELECT DepartmentId, MAX(Salary)
                  FROM Employee
                  GROUP BY DepartmentId);
```

## 练习二：换座位（难度：中等）

小美是一所中学的信息科技老师，她有一张 seat 座位表，平时用来储存学生名字和与他们相对应的座位 id。其中纵列的**id**是连续递增的。小美想改变相邻俩学生的座位。你能不能帮她写一个 SQL query 来输出小美想要的结果呢？

表的创建 + SQL基本查询语句

请创建如下所示seat表：

**示例：**

```sql
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

假如数据输入的是上表，则输出结果如下：

```sql
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```

**注意：**如果学生人数是奇数，则不需要改变最后一个同学的座位。

```sql
-- 创建stu_seat表
CREATE TABLE stu_seat(
 id INTEGER NOT NULL,
 student VARCHAR(20) NOT NULL,
 PRIMARY KEY (id)
 );
 
INSERT INTO seat VALUES(1, 'Abbot');
INSERT INTO seat VALUES(2, 'Doris');
INSERT INTO seat VALUES(3, 'Emerson');
INSERT INTO seat VALUES(4, 'Green');
INSERT INTO seat VALUES(5, 'Jeames');

-- SQL查询语句：
-- IF(判断条件，真时返回值，假时返回值)
-- 这里的第一个IF的假时返回值嵌套了另一个IF
SELECT (
    	IF
        ( S.id%2 = 0, S.id-1, IF(S.id = S1.largest, S.id, S.id+1))
       ) AS id, S.student
FROM seat AS S, (SELECT COUNT(id) AS largest FROM seat) AS S1
ORDER BY id ASC;
```

## 练习三：分数排名（难度：中等）

编写一个 SQL 查询来实现分数排名。如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。

表的创建 + SQL基本查询语句

创建以下score表：

```sql
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```

例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：

```sql
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

```sql
CREATE TABLE stu_rank (
 id INTEGER NOT NULL,
 Score FLOAT(4,2) NOT NULL,
 PRIMARY KEY (id)
 );
 
INSERT INTO score VALUES(1, 3.50);
INSERT INTO score VALUES(2, 3.65);
INSERT INTO score VALUES(3, 4.00);
INSERT INTO score VALUES(4, 3.85);
INSERT INTO score VALUES(5, 4.00);
INSERT INTO score VALUES(6, 3.65);

SELECT Score, DENSE_RANK() OVER (ORDER BY Score DESC) AS 'RANK'
FROM score;
```

## 练习四：连续出现的数字（难度：中等）

编写一个 SQL 查询，查找所有至少连续出现三次的数字。

SQL基本查询语句

```sql
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

例如，给定上面的 Logs 表， 1 是唯一连续出现至少三次的数字。

```sql
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

```sql
CREATE TABLE number (
 Id INTEGER NOT NULL,
 Num INTEGER NOT NULL,
 PRIMARY KEY (id)
 );
 
INSERT INTO number VALUES(1, 1);
INSERT INTO number VALUES(2, 1);
INSERT INTO number VALUES(3, 1);
INSERT INTO number VALUES(4, 2);
INSERT INTO number VALUES(5, 1);
INSERT INTO number VALUES(6, 2);
INSERT INTO number VALUES(7, 2);

SELECT question_id AS 'survey_log'
FROM(
	SELECT question_id, 
    	(SUM(CASE WHEN 'action' like 'answer' THEN 1 ELSE 0 END)/SUM(CASE WHEN 'action' like 'show' THEN 1 ELSE 0 END)       
   		) AS rate
    FROM survey_log
    GROUP BY question_id
    ORDER BY rate DESC
    -- 这里只选择降序排列的第一个，即为回答率最高的
    LIMIT 1
) AS S;
```

## 练习五：树节点 （难度：中等）

对于**tree**表，*id*是树节点的标识，*p_id*是其父节点的*id*。

```sql
+----+------+
| id | p_id |
+----+------+
| 1  | null |
| 2  | 1    |
| 3  | 1    |
| 4  | 2    |
| 5  | 2    |
+----+------+
```

每个节点都是以下三种类型中的一种：

* Leaf: 如果节点是根节点。
* Root: 如果节点是叶子节点。
* Inner: 如果节点既不是根节点也不是叶子节点。

写一条查询语句打印节点id及对应的节点类型。按照节点id排序。上面例子的对应结果为：

SQL基本查询语句（SQL编程）

```sql
+----+------+
| id | Type |
+----+------+
| 1  | Root |
| 2  | Inner|
| 3  | Leaf |
| 4  | Leaf |
| 5  | Leaf |
+----+------+
```

**说明**

* 节点’1’是根节点，因为它的父节点为NULL，有’2’和’3’两个子节点。
* 节点’2’是内部节点，因为它的父节点是’1’，有子节点’4’和’5’。
* 节点’3’，‘4’，'5’是叶子节点，因为它们有父节点但没有子节点。

下面是树的图形：

        1         
      /   \ 
     2    3    
    / \
    4  5

**注意：**如果一个树只有一个节点，只需要输出根节点属性。

```sql
CREATE TABLE tree (
 id INTEGER NOT NULL,
 p_id INTEGER ,
 PRIMARY KEY (id)
 );
 
INSERT INTO tree VALUES(1, NULL);
INSERT INTO tree VALUES(2, 1);
INSERT INTO tree VALUES(3, 1);
INSERT INTO tree VALUES(4, 2);
INSERT INTO tree VALUES(5, 2);

-- 结合NOT IN作为判断
SELECT id, 
	   (CASE
        WHEN (SELECT count(*) FROM tree) = 1 THEN 'Root'
        WHEN p_id is NULL THEN 'Root'
        WHEN id NOT IN (SELECT DISTINCT T1.id
                        FROM tree AS T1, tree AS T2
                        WHERE T1.id = T2.p_id) THEN 'Leaf'
        ELSE 'Inner'
        END) AS Type
FROM tree;
```

## 练习六：至少有五名直接下属的经理 （难度：中等）

**Employee**表包含所有员工及其上级的信息。每位员工都有一个Id，并且还有一个对应主管的Id（ManagerId）。

```sql
+------+----------+-----------+----------+
|Id    |Name 	  |Department |ManagerId |
+------+----------+-----------+----------+
|101   |John 	  |A 	      |null      |
|102   |Dan 	  |A 	      |101       |
|103   |James 	  |A 	      |101       |
|104   |Amy 	  |A 	      |101       |
|105   |Anne 	  |A 	      |101       |
|106   |Ron 	  |B 	      |101       |
+------+----------+-----------+----------+
```

针对**Employee**表，写一条SQL语句找出有5个下属的主管。对于上面的表，结果应输出：

```sql
+-------+
| Name  |
+-------+
| John  |
+-------+
```

**注意:**没有人向自己汇报。

## 练习七: 分数排名  （难度：中等）

练习三的分数表，实现排名功能，但是排名需要是非连续的，如下：

```sql
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 3    |
| 3.65  | 4    |
| 3.65  | 4    |
| 3.50  | 6    |
+-------+------
```

```sql
CREATE TABLE employee(
Id INTEGER NOT NULL,
Name VARCHAR(10) NOT NULL,
Department CHAR(1) NOT NULL,
ManagerId INTEGER,
PRIMARY KEY (Id)
);

INSERT INTO employee VALUES(101, 'John', 'A', NULL);
INSERT INTO employee VALUES(102, 'Dan', 'A', 101);
INSERT INTO employee VALUES(103, 'James', 'A', 101);
INSERT INTO employee VALUES(104, 'Amy', 'A', 101);
INSERT INTO employee VALUES(105, 'Anne', 'A', 101);
INSERT INTO employee VALUES(106, 'Ron', 'B', 101);


-- 利用IN + 子查询 + HAVING
SELECT Name
FROM employee
WHERE Id IN (SELECT ManagerId
             FROM employee
             GROUP BY ManagerId
             HAVING (COUNT(*) >= 5)  		
			);
```

## 练习八：查询回答率最高的问题 （难度：中等）

求出**survey_log**表中回答率最高的问题，表格的字段有：**uid, action, question_id, answer_id, q_num, timestamp**。

uid是用户id；action的值为：“show”， “answer”， “skip”；当action是"answer"时，answer_id不为空，相反，当action是"show"和"skip"时为空（null）；q_num是问题的数字序号。

写一条sql语句找出回答率最高的问题。

**举例：**

**输入**

| uid  | action | question_id | answer_id | q_num | timestamp |
| :--- | :----- | :---------- | :-------- | :---- | :-------- |
| 5    | show   | 285         | null      | 1     | 123       |
| 5    | answer | 285         | 124124    | 1     | 124       |
| 5    | show   | 369         | null      | 2     | 125       |
| 5    | skip   | 369         | null      | 2     | 126       |

**输出**

| survey_log |
| :--------- |
| 285        |

**说明**

问题285的回答率为1/1，然而问题369的回答率是0/1，所以输出是285。

**注意：**最高回答率的意思是：同一个问题出现的次数中回答的比例。

## 练习九：各部门前3高工资的员工（难度：中等）

将项目7中的employee表清空，重新插入以下数据（其实是多插入5,6两行）：

```sql
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
+----+-------+--------+--------------+
```

编写一个 SQL 查询，找出每个部门工资前三高的员工。例如，根据上述给定的表格，查询结果应返回：

```sql
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

此外，请考虑实现各部门前N高工资的员工功能。

```sql
INSERT INTO Employee VALUES(5, 'Janet', 69000, 1);
INSERT INTO Employee VALUES(6, 'Randy', 85000, 1);

-- 使用窗口函数的PARTITION + ORDER DESC，获取各部门的倒序工资
-- 只要按各部门的ranking筛选前几就可以了
SELECT T.Department, T.Employee, T.Salary
FROM (SELECT D.Name AS 'Department', E.Name AS 'Employee', E.Salary AS 'Salary',
      RANK() OVER (PARTITION BY E.DepartmentId ORDER BY E.Salary DESC) AS ranking
      FROM Employee AS E INNER JOIN Department AS D ON E.DepartmentId = D.Id
) AS  T
WHERE T.ranking <=3;
-- 各部门前N高工资的员工功能
-- WHERE T.ranking <=n
```

## 练习十：平面上最近距离 (难度: 困难）

**point_2d**表包含一个平面内一些点（超过两个）的坐标值（x，y）。

写一条查询语句求出这些点中的最短距离并保留2位小数。

```plain
|x   | y  |
|----|----|
| -1 | -1 |
|  0 |  0 |
| -1 | -2 |
```

最短距离是1，从点（-1，-1）到点（-1，2）。所以输出结果为：

| shortest |

1.00

```plain
+--------+
|shortest|
+--------+
|1.00    |
+--------+
```

**注意：**所有点的最大距离小于10000。

```sql
CREATE TABLE point_2d(
id INTEGER,
x INTEGER,
y INTEGER);

INSERT INTO point_2d VALUES(1, -1, -1);
INSERT INTO point_2d VALUES(2, 0, 0);
INSERT INTO point_2d VALUES(3, -1, -2);

-- 计算欧拉距离 SQRT((x1-x2)^2 + (y1-y2)^2)
SELECT MIN(SQRT(POW((P1.x-P2.x), 2) + POW((P1.y-P2.y), 2))) AS 'shortest'
FROM point_2d AS P1, point_2d AS P2
WHERE P1.id <> P2.id;
```

## 练习十一：行程和用户（难度：困难）

Trips 表中存所有出租车的行程信息。每段行程有唯一键 Id，Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。Status 是枚举类型，枚举成员为 (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’)。

| Id   | Client_Id | Driver_Id | City_Id | Status              | Request_at |
| :--- | :-------- | :-------- | :------ | :------------------ | :--------- |
| 1    | 1         | 10        | 1       | completed           | 2013-10-1  |
| 2    | 2         | 11        | 1       | cancelled_by_driver | 2013-10-1  |
| 3    | 3         | 12        | 6       | completed           | 2013-10-1  |
| 4    | 4         | 13        | 6       | cancelled_by_client | 2013-10-1  |
| 5    | 1         | 10        | 1       | completed           | 2013-10-2  |
| 6    | 2         | 11        | 6       | completed           | 2013-10-2  |
| 7    | 3         | 12        | 6       | completed           | 2013-10-2  |
| 8    | 2         | 12        | 12      | completed           | 2013-10-3  |
| 9    | 3         | 10        | 12      | completed           | 2013-10-3  |
| 10   | 4         | 13        | 12      | cancelled_by_driver | 2013-10-3  |

Users 表存所有用户。每个用户有唯一键 Users_Id。Banned 表示这个用户是否被禁止，Role 则是一个表示（‘client’, ‘driver’, ‘partner’）的枚举类型。

```sql
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
```

写一段 SQL 语句查出**2013年10月1日**至**2013年10月3日**期间非禁止用户的取消率。基于上表，你的 SQL 语句应返回如下结果，取消率（Cancellation Rate）保留两位小数。

```sql
+------------+-------------------+
|     Day    | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |       0.33        |
| 2013-10-02 |       0.00        |
| 2013-10-03 |       0.50        |
+------------+-------------------+
```

```sql
CREATE TABLE Trips(
Id INTEGER NOT NULL,
Client_Id INTEGER,
Driver_Id INTEGER,
City_Id  INTEGER,
Status VARCHAR(30),
Request_at DATE,
PRIMARY KEY(Id)    
);

INSERT INTO Trips VALUES(1, 1, 10, 1, 'completed', '2013-10-1');
INSERT INTO Trips VALUES(2, 2, 11, 1, 'cancelled_by_driver', '2013-10-1');
INSERT INTO Trips VALUES(3, 3, 12, 6, 'completed', '2013-10-1');
INSERT INTO Trips VALUES(4, 4, 13, 6, 'cancelled_by_client', '2013-10-1');
INSERT INTO Trips VALUES(5, 1, 10, 1, 'completed', '2013-10-2');
INSERT INTO Trips VALUES(6, 2, 11, 6, 'completed', '2013-10-2');
INSERT INTO Trips VALUES(7, 3, 12, 6, 'completed', '2013-10-2');
INSERT INTO Trips VALUES(8, 2, 12, 12, 'completed', '2013-10-3');
INSERT INTO Trips VALUES(9, 3, 10, 12, 'completed', '2013-10-3');
INSERT INTO Trips VALUES(10, 4, 13, 12, 'cancelled_by_driver', '2013-10-3');


CREATE TABLE Users(
Users_Id INTEGER NOT NULL,
Banned VARCHAR(5) NOT NULL,
Role VARCHAR(10) NOT NULL,
PRIMARY KEY(Users_Id)     
);

INSERT INTO Users VALUES(1,'No','client');
INSERT INTO Users VALUES(2,'YES','client');
INSERT INTO Users VALUES(3,'No','client');
INSERT INTO Users VALUES(4,'No','client');
INSERT INTO Users VALUES(10,'No','driver');
INSERT INTO Users VALUES(11,'No','driver');
INSERT INTO Users VALUES(12,'No','driver');
INSERT INTO Users VALUES(13,'No','driver');

-- 题目要求：
-- 查出2013年10月1日至2013年10月3日期间
-- 非禁止用户
-- 的取消率。

-- 首先按日期筛选
-- 依次和client、driver进行连结
-- 然后进行非banned的筛选
-- 最后GROUP BY按天进行分组
-- 计算取消率，可用CASE或者IF
-- ROUND(SUM(CASE (Status != 'completed') THEN 1 ELSE 0 END))/COUNT(*), 2) AS 'Cancellation Rate'

SELECT Request_at AS 'Day',
		ROUND(SUM(IF(Status != 'completed', 1, 0)) / COUNT(*), 2) AS 'Cancellation Rate'
FROM (SELECT * FROM Trips WHERE Request_at BETWEEN '2013-10-1' AND '2013-10-3') AS T
INNER JOIN (SELECT * FROM Users WHERE ROLE='client') AS C ON T.Client_Id = C.Users_Id
INNER JOIN (SELECT * FROM Users WHERE ROLE='driver') AS D ON T.Driver_Id = D.Users_Id
WHERE C.Banned != 'Yes' AND D.Banned != 'Yes'
GROUP BY Request_at;
```
