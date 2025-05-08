---

title: Mysql练习 暑假篇
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.22
tags: 
   - mysql 

---

# Mysql练习

## 基础题

## 1.

从INS_SPR中选择SPR_FNM1 、SPR_SURN，其中SPR_CODE = '50200100'  

```sql
SELECT SPR_FNM1 , SPR_SURN FROM INS_SPR WHERE SPR_CODE = 50200100;
```

return:

| SPR_FNM1 | SPR_SURN |
| -------- | -------- |
| Tom      | Cotton   |

## 2.

显示学生编号为 50200100 的学生在 2016/7 TR1 学期学习的模块代码和模块名称

```sql
SELECT CAM_SMO.MOD_CODE,INS_MOD.MOD_NAME
FROM INS_MOD JOIN CAM_SMO ON (INS_MOD.MOD_CODE=CAM_SMO.MOD_CODE)
WHERE CAM_SMO.SPR_CODE = '50200100'
AND CAM_SMO.AYR_CODE = '2016/7'
AND CAM_SMO.PSL_CODE = 'TR1';
```

| 修改代码  | 模组名称   |
| --------- | ---------- |
| CSN08101  | 系统和服务 |
| 信息08104 | 数据库系统 |
| SET08108  | 软件开发 2 |

## 3.

**显示学生编号为 50200100 的学生在 2016/7 TR1 学期学习的模块代码、模块名称和模块负责人的详细信息**

```sql
SELECT CAM_SMO.MOD_CODE, INS_MOD.MOD_NAME,
       INS_PRS.PRS_CODE, INS_PRS.PRS_FNM1, INS_PRS.PRS_SURN
  FROM CAM_SMO JOIN INS_MOD ON (INS_MOD.MOD_CODE=CAM_SMO.MOD_CODE)
               JOIN INS_PRS ON (INS_MOD.PRS_CODE=INS_PRS.PRS_CODE)
 WHERE CAM_SMO.SPR_CODE='50200100'
   AND CAM_SMO.AYR_CODE='2016/7'
   AND CAM_SMO.PSL_CODE='TR1'

```

return:

| 修改代码  | 模组名称   | PRS 代码 | PRS_FNM1 | PRS 查询 |
| --------- | ---------- | -------- | -------- | -------- |
| CSN08101  | 系统和服务 | 40000008 | 詹姆士   | 杰克逊   |
| 信息08104 | 数据库系统 | 40000036 | 安德鲁   | 卡明     |
| SET08108  | 软件开发 2 | 40000408 | 尼尔     | 厄克特   |

## 4.力扣sql 175组合两个表

```
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId
;

```

## 5.leetcode [超过经理收入的员工](https://leetcode.cn/problems/employees-earning-more-than-their-managers/)

表：`Employee` 

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |
+-------------+---------+
id 是该表的主键（具有唯一值的列）。
该表的每一行都表示雇员的ID、姓名、工资和经理的ID。
```

 

编写解决方案，找出收入比经理高的员工。

以 **任意顺序** 返回结果表。

结果格式如下所示。

```
输入: 
Employee 表:
+----+-------+--------+-----------+
| id | name  | salary | managerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | Null      |
| 4  | Max   | 90000  | Null      |
+----+-------+--------+-----------+
输出: 
+----------+
| Employee |
+----------+
| Joe      |
+----------+
解释: Joe 是唯一挣得比经理多的雇员。
```

```
题解：
select a.Name as 'Employee'
from Employee as a,Employee as b
where a.ManagerId = b.Id
AND a.Salary > b.Salary;
```

这是表的自连接，通过同一张表的不同列的对比

首先的条件是

a.ManagerId = b.Id这个表明这个员工归属这个经理，然后a.Salary > b.Salary;这个条件表明这个员工的工资大于他的经理

## 6.leetcode [查找重复的电子邮箱](https://leetcode.cn/problems/duplicate-emails/description/)

```
表: Person

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id 是该表的主键（具有唯一值的列）。
此表的每一行都包含一封电子邮件。电子邮件不包含大写字母。
 

编写解决方案来报告所有重复的电子邮件。 请注意，可以保证电子邮件字段不为 NULL。

以 任意顺序 返回结果表。

结果格式如下例。

 

示例 1:

输入: 
Person 表:
+----+---------+
| id | email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
输出: 
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
解释: a@b.com 出现了两次。
```

题解：

第一种思路，通过条件筛选

```
SELECT DISTINCT a.email AS 'Email'
FROM Person a, Person b
WHERE a.email = b.email AND a.id != b.id;

```

`DISTINCT` 关键字用于选择不同的（不重复的）值。

使用了自连接 `Person a, Person b`。

条件 `a.email = b.email` 用于匹配相同的电子邮件。

条件 `a.id < b.id` 确保每对重复的电子邮件只记录一次，避免重复。

第二种思路，使用分组

```
SELECT
    email
FROM
    Person
GROUP BY
    email
HAVING
    count(email) > 1
```

使用分组直接显示出email大于一的邮箱的次数

## 7.leetcode [删除重复的电子邮箱](https://leetcode.cn/problems/delete-duplicate-emails/)



```
表: Person

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id 是该表的主键列(具有唯一值的列)。
该表的每一行包含一封电子邮件。电子邮件将不包含大写字母。
 

编写解决方案 删除 所有重复的电子邮件，只保留一个具有最小 id 的唯一电子邮件。

（对于 SQL 用户，请注意你应该编写一个 DELETE 语句而不是 SELECT 语句。）

（对于 Pandas 用户，请注意你应该直接修改 Person 表。）

运行脚本后，显示的答案是 Person 表。驱动程序将首先编译并运行您的代码片段，然后再显示 Person 表。Person 表的最终顺序 无关紧要 。

返回结果格式如下示例所示。

 

示例 1:

输入: 
Person 表:
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
输出: 
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
解释: john@example.com重复两次。我们保留最小的Id = 1。
```



题解：

```sql
DELETE p1 FROM Person p1,
    Person p2
WHERE
    p1.Email = p2.Email AND p1.Id > p2.Id

```

8.leetcode [上升的温度](https://leetcode.cn/problems/rising-temperature/)

```
表： Weather

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id 是该表具有唯一值的列。
没有具有相同 recordDate 的不同行。
该表包含特定日期的温度信息
 

编写解决方案，找出与之前（昨天的）日期相比温度更高的所有日期的 id 。

返回结果 无顺序要求 。

结果格式如下例子所示。

 

示例 1：

输入：
Weather 表：
+----+------------+-------------+
| id | recordDate | Temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
输出：
+----+
| id |
+----+
| 2  |
| 4  |
+----+
解释：
2015-01-02 的温度比前一天高（10 -> 25）
2015-01-04 的温度比前一天高（20 -> 30）
```

题解：

一开始犯了个错误，直接用+1是显然不行的

去查了一遍日期函数，发现还是有不少函数可以用

索引可以得到

```
SELECT nw.id 
FROM Weather w,Weather nw
WHERE DATEDIFF(nw.recordDate,w.recordDate)=1 
AND w.Temperature < nw.Temperature

```

思路扩展：

```
SELECT w1.id
FROM Weather w1
JOIN Weather w2 ON w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE w1.temperature > w2.temperature;

```

w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)

使用日期函数来显示这个条件

## 9.leetcode [游戏玩法分析 I](https://leetcode.cn/problems/game-play-analysis-i/)

```
活动表 Activity：

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
在 SQL 中，表的主键是 (player_id, event_date)。
这张表展示了一些游戏玩家在游戏平台上的行为活动。
每行数据记录了一名玩家在退出平台之前，当天使用同一台设备登录平台后打开的游戏的数目（可能是 0 个）。
 

查询每位玩家 第一次登录平台的日期。

查询结果的格式如下所示：

Activity 表：
+-----------+-----------+------------+--------------+
| player_id | device_id | event_date | games_played |
+-----------+-----------+------------+--------------+
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |
+-----------+-----------+------------+--------------+

Result 表：
+-----------+-------------+
| player_id | first_login |
+-----------+-------------+
| 1         | 2016-03-01  |
| 2         | 2017-06-25  |
| 3         | 2016-03-02  |
+-----------+-------------+
```

题解：

首先我是想直接多表查询，然后用id相等来确定是同一个人，然后直接a1.event_date < a2.event_date就行了，但是发现这样并不能完全正确。因为这样的话就会缺少一个id=2的人，发现他只有一次

然后我就改了一版

```
select a1.player_id ,a1.event_date first_login from Activity a1 ,Activity a2
where (a1.player_id = a2.player_id or (a1.player_id <> null and a2.player_id = null))
and a1.event_date < a2.event_date;
```

发现这样还是缺了一部分

然后看了看题解

发现了这样一种思路

```
select player_id, min(event_date) as first_login
from Activity
group by player_id;

```

发现是我想多了哈哈，这个题根本不用那么难的/(ㄒoㄒ)/~~

直接检索最小值，然后根据id分组就行了

## 10.lintcode [整合成绩单](https://www.lintcode.com/problem/3616/?showListFe=true&page=1&problemTypeId=3&pageSize=50)

描述

现在有两张结构相同的成绩表 `course1_score` 和 `course2_score`，请编写 SQL 语句，找出两张表中都存在的学生姓名，在结果表格中通过 `score1` 列和 `score2` 列分别展示这些学生的两门课程成绩，**并按照姓名的字典序进行升序排序。**

表定义1：`course1_score`（课程1的成绩表）

| 列名  |     类型     |    注释     |
| :---: | :----------: | :---------: |
|  id   | int unsigned |    主键     |
| name  |   varchar    |  学生姓名   |
| score |     int      | 课程1的成绩 |

表定义2：`course2_score`（课程2的成绩表）

| 列名  |     类型     |    注释     |
| :---: | :----------: | :---------: |
|  id   | int unsigned |    主键     |
| name  |   varchar    |  学生姓名   |
| score |     int      | 课程2的成绩 |

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取



样例

**样例一：**

表内容1：`course1_score`

|  id  |  name   | score |
| :--: | :-----: | :---: |
|  1   |  Alice  |  100  |
|  2   |   Bob   |  90   |
|  3   | Chalice |  95   |
|  4   |  David  |  90   |
|  5   | Edward  |  85   |

表内容2：`course2_score`

|  id  |  name   | score |
| :--: | :-----: | :---: |
|  1   | Chalice |  98   |
|  2   |  Bill   |  90   |
|  3   |  Alice  |  95   |
|  4   |  Ethan  |  85   |

在运行你的 SQL 语句之后，表应返回：

|  name   | score1 | score2 |
| :-----: | :----: | :----: |
|  Alice  |  100   |   95   |
| Chalice |   95   |   98   |

**样例二：**

表内容1：`course1_score`

|  id  |  name   | score |
| :--: | :-----: | :---: |
|  1   |  Alice  |  100  |
|  2   |   Bob   |  90   |
|  3   | Chalice |  95   |
|  4   |  David  |  90   |
|  5   | Edward  |  85   |

表内容2：`course2_score`

|  id  | name  | score |
| :--: | :---: | :---: |
|  1   | Carl  |  98   |
|  2   | Bill  |  90   |
|  3   |  Ali  |  95   |
|  4   | Ethan |  85   |

在运行你的 SQL 语句之后，表应返回：

| name | score1 | score2 |
| :--: | :----: | :----: |
|      |        |        |

题解：

因为他用加粗标出来了，需要按照name字段进行升序，所以要使用order by 。然后进行多表查询，通过筛选条件

```
c1.name , c1.score c2.score from course1_score c1 , course2_score c2 
where c1.name = c2.name 
order by c1.name;
```

一开始我是这样，基本正确，但是不能通过🤯

因为一开始的话就要声明🤦‍

```
SELECT c1.name, c1.score AS score1, c2.score AS score2 
FROM course1_score c1, course2_score c2 
WHERE c1.name = c2.name 
ORDER BY c1.name;

```

## 11 lintcode [耗时前三的任务](https://lintcode.com/problem/3618/?showListFe=true&page=1&problemTypeId=3&pageSize=50)

描述

在本题中，你需要根据 `Tasks` 表：

| column     | type |
| :--------- | :--- |
| id         | int  |
| start_date | date |
| end_date   | date |

找到任务耗时排名前三的任务（三条以下的记录只返回前 n 条）。你可以在样例中查看输出需要的格式。

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取





- **如果存在多个消耗时间相同的任务，优先返回 `id` 较小的任务**

样例

**样例 1**

输入：

| id   | start_date | end_date   |
| :--- | :--------- | :--------- |
| 0    | 2019-01-06 | 2019-04-16 |
| 1    | 2022-09-28 | 2022-12-29 |
| 2    | 2020-07-25 | 2020-09-29 |
| 3    | 2018-02-12 | 2018-02-27 |
| 4    | 2022-08-20 | 2022-11-18 |

输出：

| id   | diff |
| :--- | :--- |
| 0    | 100  |
| 1    | 92   |
| 4    | 90   |

**样例 2**

输入：

| id   | start_date | end_date   |
| :--- | :--------- | :--------- |
| 0    | 2019-01-06 | 2019-04-16 |
| 1    | 2022-09-28 | 2022-12-29 |

输出：

| id   | diff |
| :--- | :--- |
| 0    | 100  |
| 1    | 92   |

题解：

```
select id,datediff(end_date, start_date) as diff
from Tasks
order by diff desc
limit 3;
```

这个题目主要是用了排序的提交，还有分页，最重要的是日期函数datediff计算两个日期之间间隔的日子

一开始我没看到题目要求，忘了limit了提交不成功🤯

## 12 lintcode [查询客户的推荐人](https://www.lintcode.com/problem/3614/?showListFe=true&page=1&problemTypeId=3&level=1&pageSize=50)

描述

给定表 `customer` ，里面存储了所有客户信息和他们的推荐人，具体如下所示。

| 列名        | 类型    | 说明                |
| :---------- | :------ | :------------------ |
| id          | INT     | 客户的 id，**主键** |
| name        | VARCHAR | 客户的名称          |
| referrer_id | INT     | 推荐人的 id         |

现需要你写一个查询语句，返回一个**只包含 `name` 字段的客户列表**，要求列表中客户的推荐人编号都**不是 1**。

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取





`referrer_id` 可能会是 *null*，**该情况也不是 1**。

样例

**样例一**

输入数据

| id   | name                | referrer_id |
| :--- | :------------------ | :---------- |
| 1    | Joanne Ferry        | null        |
| 2    | Roberta Nader Sr.   | 2           |
| 3    | Dr. Gwen Jacobson   | 1           |
| 4    | Geraldine Kiehn III | 2           |
| 5    | Gwendolyn Howe IV   | 2           |

输出结果

| name                |
| :------------------ |
| Joanne Ferry        |
| Roberta Nader Sr.   |
| Geraldine Kiehn III |
| Gwendolyn Howe IV   |

**样例二**

输入数据

| id   | name              | referrer_id |
| :--- | :---------------- | :---------- |
| 1    | Joanne Ferry      | null        |
| 2    | Roberta Nader Sr. | 1           |
| 3    | Dr. Gwen Jacobson | 2           |

输出结果

| name              |
| :---------------- |
| Joanne Ferry      |
| Dr. Gwen Jacobson |

题解：

```
select name from customer
where referrer_id is NULL or referrer_id <> 1;
```

直接筛选就行，他这个题目好像出bug了，我的东西都是正确的但是他给我输出错误😊

13. ## lintcode [· 查询成绩排名在第二到第五的学生](https://www.lintcode.com/problem/3613/?showListFe=true&page=1&problemTypeId=3&pageSize=50)

描述

请编写 SQL 语句，从成绩表 `score` 中查询所有学生的三门课程的总分排名，并返回排名 `score_rank` 在第二到第五的学生学号 `sno` 以及这名学生的总分 `total_score`，**排名不设置并列排名，当总分相同时，学号 `sno` 小的学生排在前面。**

表定义：`score`（成绩表）

|  列名   |     类型     |    注释     |
| :-----: | :----------: | :---------: |
|   id    | int unsigned |    主键     |
|   sno   |   varchar    |  学生学号   |
| course1 |     int      | 课程1的分数 |
| course2 |     int      | 课程2的分数 |
| course3 |     int      | 课程3的分数 |

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取





当学生数量不足 5 人时，返回包括第 2 名之后的剩余学生的成绩排名表格。

样例

**样例一：**
表内容：`score`

|  id  | sno  | course1 | course2 | course3 |
| :--: | :--: | :-----: | :-----: | :-----: |
|  1   | 001  |   80    |   90    |   70    |
|  2   | 002  |   90    |   85    |   80    |
|  3   | 003  |   85    |   70    |   95    |
|  4   | 004  |   80    |   90    |   85    |
|  5   | 005  |   60    |   80    |   75    |
|  6   | 006  |   90    |   75    |   70    |

在运行你的 SQL 语句之后，表应返回：

| score_rank | sno  | total_score |
| :--------: | :--: | :---------: |
|     2      | 004  |     255     |
|     3      | 003  |     250     |
|     4      | 001  |     240     |
|     5      | 006  |     235     |

学号为 `002` 和 `004` 的学生总分都为 `255`，但学号 `002` 较小，因此排在第一，而学号为 `004` 的学生排在第二。

**样例二：**
表内容：`score`

|  id  | sno  | course1 | course2 | course3 |
| :--: | :--: | :-----: | :-----: | :-----: |
|  1   | 001  |   80    |   90    |   70    |

在运行你的 SQL 语句之后，表应返回：

| score_rank | sno  | total_score |
| :--------: | :--: | :---------: |
|            |      |             |

没有排名从第二到第五的学生，返回空表。

**样例三：**
表内容：`score`

|  id  | sno  | course1 | course2 | course3 |
| :--: | :--: | :-----: | :-----: | :-----: |
|  1   | 001  |   80    |   90    |   70    |
|  2   | 002  |   85    |   70    |   95    |
|  3   | 003  |   80    |   90    |   85    |

在运行你的 SQL 语句之后，表应返回：

| score_rank | sno  | total_score |
| :--------: | :--: | :---------: |
|     2      | 002  |     245     |
|     3      | 001  |     240     |

只有三名学生，返回第二到第三的学生。

题解：

```
SELECT row_number() over() as score_rank, sno, (course1 + course2 + course3) as total_score
from score
order by total_score desc
limit 1,4

```

使用`ROW_NUMBER()` 函数来计算排名，

**补充：在 SQL 中，`OVER` 子句与窗口函数一起使用，用于定义窗口或集合，窗口函数在该窗口或集合上执行计算。常见的窗口函数包括 `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, 和 `SUM()` 等。**

- **`**RANK()`：会为相同值的行分配相同的排名，并在不同值之间留出空缺。**
- **`DENSE_RANK()`：会为相同值的行分配相同的排名，但不会在不同值之间留出空缺。**

**选择使用哪一个函数取决于你的需求：如果需要连续的排名（无空缺），使用 `DENSE_RANK()`；如果允许空缺，使用 `RANK()`。**

最后通过limit来限制出现2-5名即可

14. ## lintcode [所有学生都选修的课程](https://www.lintcode.com/problem/3610/?showListFe=true&page=1&problemTypeId=3&pageSize=50)

    

描述

请编写 SQL 语句，从选课表 `courses` 中，查询选课表里存在的学生中，所有学生都选修的课程编号 `course_id`，并将结果 `course_id` 进行升序排序。

表定义：`courses`（选课表）

|    列名    |     类型     |   注释   |
| :--------: | :----------: | :------: |
|     id     | int unsigned |   主键   |
| student_id |     int      | 学生学号 |
| course_id  |   varchar    | 课程编号 |

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取



样例

**样例一：**

表内容：`courses`

|  id  | student_id | course_id |
| :--: | :--------: | :-------: |
|  1   |     1      |    101    |
|  2   |     1      |    102    |
|  3   |     1      |    103    |
|  4   |     2      |    101    |
|  5   |     2      |    103    |
|  6   |     4      |    103    |

在运行你的 SQL 语句之后，表应返回：

| course_id |
| :-------: |
|    103    |

解释：表中记录了学号为 1、2、4 的学生的选课情况，这三名学生都选修了课程编号为 `103` 的课程。

**样例二：**

表内容：`courses`

|  id  | student_id | course_id |
| :--: | :--------: | :-------: |
|  1   |     1      |    101    |
|  2   |     2      |    101    |
|  3   |     1      |    102    |
|  4   |     2      |    102    |
|  5   |     3      |    103    |

在运行你的 SQL 语句之后，表应返回：

| course_id |
| :-------: |
|           |

解释：表中记录了学号为 1、2、3 的学生的选课情况，没有一门课程是这三名学生都选修的，故返回空表。

```
select course_id
from courses
group by course_id
having count(distinct student_id) = (select count(distinct student_id) from courses)
```

having count(distinct student_id) = (select count(distinct student_id) from courses)这个选出学生id数量和课程数量相同的

然后进行分组，就可以选出其所有学生都选的课

15. ## lintcode [ 考试通过的人数](https://www.lintcode.com/problem/3607/description?showListFe=true&page=1&problemTypeId=3&level=1&pageSize=50)



描述

请编写 SQL 语句，从学生表 `students` 和成绩表 `score` 中查询各个班级 `class` 中，**三个课程都大于等于 60 分**的学生数量 `pass_count`。

表定义1：`students`（学生表）

| 列名  |     类型     |   注释   |
| :---: | :----------: | :------: |
|  id   | int unsigned |   主键   |
|  sno  |   varchar    | 学生学号 |
| name  |   varchar    | 学生姓名 |
| class |   varchar    | 学生班级 |

表定义2：`score`（成绩表）

|  列名   |     类型     |    注释     |
| :-----: | :----------: | :---------: |
|   id    | int unsigned |    主键     |
|   sno   |   varchar    |  学生学号   |
| course1 |     int      | 课程1的分数 |
| course2 |     int      | 课程2的分数 |
| course3 |     int      | 课程3的分数 |
|  total  |     int      |    总分     |

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取





需要返回 `students` 表中出现过的所有班级的 `pass_count`，若该班级没有符合条件的学生，则 `pass_count` 为 **0 。**

样例

**样例一：**

表内容 1：`students`

|  id  | sno  |       name       | class |
| :--: | :--: | :--------------: | :---: |
|  1   | 001  |  Martin Torphy   | CS01  |
|  2   | 002  | Eleanor Cummings | CS01  |
|  3   | 003  |  Franklin Upton  | CS02  |
|  4   | 004  |   Toby Hudson    | CS02  |

表内容 2：`score`

|  id  | sno  | course1 | course2 | course3 | total |
| :--: | :--: | :-----: | :-----: | :-----: | :---: |
|  1   | 003  |   100   |   100   |   100   |  300  |
|  2   | 002  |   90    |   90    |   100   |  280  |
|  3   | 004  |   80    |   60    |   70    |  210  |
|  4   | 001  |   59    |   60    |   60    |  179  |

在运行你的 SQL 语句之后，表应返回：

| class | pass_count |
| :---: | :--------: |
| CS01  |     1      |
| CS02  |     2      |

**样例二：**

表内容 1：`students`

|  id  | sno  |       name       | class |
| :--: | :--: | :--------------: | :---: |
|  1   | 001  |  Martin Torphy   | CS02  |
|  2   | 002  | Eleanor Cummings | CS02  |
|  3   | 003  |  Franklin Upton  | CS02  |
|  4   | 004  |   Toby Hudson    | CS02  |
|  5   | 005  |    Marco Funk    | CS02  |

表内容 2：`score`

|  id  | sno  | course1 | course2 | course3 | total |
| :--: | :--: | :-----: | :-----: | :-----: | :---: |
|  1   | 005  |   100   |   100   |   100   |  300  |
|  2   | 002  |   90    |   90    |   100   |  280  |
|  3   | 004  |   90    |   90    |   90    |  270  |
|  4   | 003  |   100   |   100   |   59    |  259  |
|  5   | 001  |   59    |   59    |   59    |  177  |

在运行你的 SQL 语句之后，表应返回：

| class | pass_count |
| :---: | :--------: |
| CS02  |     3      |

题解：

```
select s1.class as class ,count(s1.class) as pass_count
from students  s1 join score  s2 on s1.sno = s2.sno
where s2.course1 >= 60 AND s2.course2 >= 60 AND s2.course3 >= 60
group by s1.class;
```

首先先要确定这是要进行多表查询，而且连接的条件是s1.sno = s2.sno这是两张表的连接条件

然后根据要求写where 语句都大于等于60分

然后根据班级分组

最后再写return返回的列表值

## 16.lintcode 最棒的销售

请你编写 SQL 语句，查询表 `sales` 表中销售额最高的销售 `id` 。
表定义：sales（销售表）

| id           | sales_sum | satisfaction |
| :----------- | :-------- | :----------- |
| id           | int       | 主键         |
| sales_sum    | int       | 销售额       |
| satisfaction | float     | 客户满意度   |

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取





如果存在多个 `id` 的销售额相同且都为最大值，则返回符合条件的所有 `id`

样例

样例一

表内容：sales

| id   | sales_sum | satisfaction |
| :--- | :-------- | :----------- |
| 1    | 50000     | 2.2          |
| 2    | 25000     | 2.5          |
| 3    | 60000     | 3.2          |

在运行你的 `SQL` 语句后，表应返回：

| id   |
| :--- |
| 3    |

样例二：
表内容：sales

| id   | sales_sum | satisfaction |
| :--- | :-------- | :----------- |
| 1    | 34560     | 2.3          |
| 2    | 12345     | 3.4          |
| 3    | 55000     | 0.9          |
| 4    | 45000     | 5            |

在运行你的 `SQL` 语句后，表应返回：

| id   |
| :--- |
| 3    |

题解：

```sql
select id from sales order by sales_sum desc limit 1;
```

## 17.lintcode  视图处理算法 Merge

请创建一个视图 `v_teachers` 查看 `teachers` 表中年龄大于25岁的教师信息，并使用 `Merge` 的视图算法

表定义 : teachers (教师表)

|  列名   |     类型     |   注释   |
| :-----: | :----------: | :------: |
|   id    | int unsigned |   主键   |
|  name   |   varchar    | 讲师姓名 |
|  email  |   varchar    | 讲师邮箱 |
|   age   |     int      | 讲师年龄 |
| country |   varchar    | 讲师国籍 |

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取



样例

**输入数据：**

`teachers` 表：

| id   | name               | email                                                        | age  | country |
| :--- | :----------------- | :----------------------------------------------------------- | :--- | :------ |
| 1    | 'Eastern heretic'  | '[eastern.heretic@gmail.com](mailto:eastern.heretic@gmail.com)' | 20   | 'UK'    |
| 2    | 'Northern Beggar'  | '[northern.beggar@qq.com](mailto:northern.beggar@qq.com)'    | 21   | 'CN'    |
| 3    | 'Western Venom'    | '[western.venom@163.com](mailto:western.venom@163.com)'      | 28   | 'USA'   |
| 4    | 'Southern Emperor' | '[southern.emperor@qq.com](mailto:southern.emperor@qq.com)'  | 21   | 'JP'    |

**返回结果：**

| id   | name            | email                                                   | age  | country |
| :--- | :-------------- | :------------------------------------------------------ | :--- | :------ |
| 3    | 'Western Venom' | '[western.venom@163.com](mailto:western.venom@163.com)' | 28   |         |

题解：

```
create algorithm = Merge view v_teachers
as 
select *
from teachers 
where age>25;
```

**主要是algorithm = Merge创建merge的视图**

18.lintcode 创建批量插入数据的存储过程

我们需要向 `teachers` 表中插入 `30000` 条测试数据，包含教师姓名 `name = 'teacher' + 测试 id`，（测试 `id 从 1 增加到 30000`），教师邮箱 `email = name + '@chapter.com'`，教师年龄 `age = 26 + (id%20)`
请利用 `SQl` 的存储过程来实现，请将存储过程命名为 `addTeachers`。

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取



样例

**输入数据：**

`teachers` 表：

| id   | name | email | age  | country |
| :--- | :--- | :---- | :--- | :------ |
|      |      |       |      |         |

**返回结果：**

| id    | name       | email                                                 | age   | country |
| :---- | :--------- | :---------------------------------------------------- | :---- | :------ |
| 1     | 'teacher1' | '[teacher1@chapter.com](mailto:teacher1@chapter.com)' | 27    | ''      |
| 2     | 'teacher2' | '[teacher2@chapter.com](mailto:teacher2@chapter.com)' | 28    | ''      |
| 3     | 'teacher3' | '[teacher3@chapter.com](mailto:teacher3@chapter.com)' | 29    | ''      |
| 4     | 'teacher4' | '[teacher4@chapter.com](mailto:teacher4@chapter.com)' | 30    | ''      |
| 5     | 'teacher5' | '[teacher5@chapter.com](mailto:teacher5@chapter.com)' | 31    | ''      |
| 6     | 'teacher6' | '[teacher6@chapter.com](mailto:teacher6@chapter.com)' | 32    | ''      |
| 7     | 'teacher7' | '[teacher7@chapter.com](mailto:teacher7@chapter.com)' | 33    | ''      |
| '...' | '...'      | '...'                                                 | '...' | '...'   |

题解：

```
CREATE PROCEDURE addTeachers()
BEGIN
    DECLARE num INT DEFAULT 0;
    WHILE num < 30000 DO
        INSERT INTO teachers (name, email, age, country)
        VALUES (CONCAT('teacher', num), CONCAT('teacher', num, '@chapter.com'), 26 + (num % 20), '');
        SET num = num + 1;
    END WHILE;
END;
```

## 18.lintcode [更换连续两个人的座位](https://www.lintcode.com/problem/3617/description?showListFe=true&page=1&problemTypeId=3&level=2&pageSize=50)

现有一表 `seat` 存储了每个座位的信息，具体如下所示。

| 列名 | 类型    | 说明                 |
| :--- | :------ | :------------------- |
| id   | INT     | 座位的编号，**主键** |
| name | VARCHAR | 使用该座位的人名     |

请编写 SQL 查询来交换**每两个连续的人的座位号**。如果总体的数量是奇数，则最后一个人的座位不进行交换。

最终的结果需要包含 `id` 和 `name` 两个字段，并按 `id` **升序**返回结果表。

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取



样例

**样例一**

输入数据

| id   | name                |
| :--- | :------------------ |
| 1    | Armando Homenick    |
| 2    | Jesus Runolfsdottir |
| 3    | Jody Hackett        |
| 4    | Brendan Legros MD   |
| 5    | Tina Cormier        |

输出结果

| id   | name                |
| :--- | :------------------ |
| 1    | Jesus Runolfsdottir |
| 2    | Armando Homenick    |
| 3    | Brendan Legros MD   |
| 4    | Jody Hackett        |
| 5    | Tina Cormier        |

解释

输入数据包含 **5** 个座位信息，则对编号 **[1 <-> 2]**、**[3 <-> 4]** 四个座位对应的人进行更换座位，`id` 为 5 的**不进行更换**。

**样例二**

输入数据

| id   | name                |
| :--- | :------------------ |
| 1    | Armando Homenick    |
| 2    | Jesus Runolfsdottir |
| 3    | Jody Hackett        |
| 4    | Brendan Legros MD   |
| 5    | Tina Cormier        |
| 6    | Raymond Metz        |

输出结果

| id   | name                |
| :--- | :------------------ |
| 1    | Jesus Runolfsdottir |
| 2    | Armando Homenick    |
| 3    | Brendan Legros MD   |
| 4    | Jody Hackett        |
| 5    | Raymond Metz        |
| 6    | Tina Cormier        |

解释

输入数据包含 **6** 个座位信息，则对编号 **[1 <-> 2]**、**[3 <-> 4]** 以及 **[5 <-> 6]** 六个座位对应的人都进行更换座位。

题解：

使用了case语句

```
SELECT 
    CASE 
        WHEN id % 2 = 1 AND id = (SELECT COUNT(*) FROM seat) THEN id
        WHEN id % 2 = 1 THEN id + 1
        ELSE id - 1
    END AS id, 
    name
FROM seat 
ORDER BY id;
```

## 19.lintcode [ 查询国籍为 'USA' 的所有教师所授课程名称](https://www.lintcode.com/problem/2063/?showListFe=true&page=1&problemTypeId=3&level=2&ordering=level&pageSize=50)

请编写 SQL 语句， 联合课程表（courses）和教师表（teachers）查询课程表中，国籍为 USA 的所有教师所授课程名称（name）。

表定义：teachers（教师表）

| 列名    | 类型         | 注释     |
| :------ | :----------- | :------- |
| id      | int unsigned | 主键     |
| name    | varchar      | 讲师姓名 |
| email   | varchar      | 讲师邮箱 |
| age     | int          | 讲师年龄 |
| country | varchar      | 讲师国籍 |

表定义：courses（课程表）

| 列名          | 类型         | 注释         |
| :------------ | :----------- | :----------- |
| id            | int unsigned | 主键         |
| name          | varchar      | 课程名称     |
| student_count | int          | 学生总数     |
| created_at    | datetime     | 课程创建时间 |
| teacher_id    | int unsigned | 讲师 id      |

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取





- 如果 teachers 中列 country 为 null，则返回的数据也为 null
- 如果 courses 中列 teacher id 为 null，则返回的数据也为 null

样例

**样例一：**

表内容 : teachers

| id   | name             | email                                                        | age  | country |
| :--- | :--------------- | :----------------------------------------------------------- | :--- | :------ |
| 1    | Eastern Heretic  | [eastern.heretic@gmail.com](mailto:eastern.heretic@gmail.com) | 20   | UK      |
| 2    | Northern Beggar  | [northern.beggar@qq.com](mailto:northern.beggar@qq.com)      | 21   | CN      |
| 3    | Western Venom    | [western.venom@163.com](mailto:western.venom@163.com)        | 28   | USA     |
| 4    | Southern Emperor | [southern.emperor@qq.com](mailto:southern.emperor@qq.com)    | 21   | JP      |
| 5    | Linghu Chong     | NULL                                                         | 18   | CN      |

表内容：courses

| **id** | **name**                | **student_count** | **created_at**     | **teacher_id** |
| :----- | :---------------------- | :---------------- | :----------------- | :------------- |
| 1      | Senior Algorithm        | 880               | 2020-6-1 09:03:12  | 4              |
| 2      | System Design           | 1350              | 2020-7-18 10:03:12 | 3              |
| 3      | Django                  | 780               | 2020-2-29 12:03:12 | 3              |
| 4      | Web                     | 340               | 2020-4-22 13:03:12 | 4              |
| 5      | Big Data                | 700               | 2020-9-11 16:03:12 | 1              |
| 6      | Artificial Intelligence | 1660              | 2018-5-13 18:03:12 | 3              |
| 7      | Java P6+                | 780               | 2019-1-19 13:03:12 | 3              |
| 8      | Data Analysis           | 500               | 2019-7-12 13:03:12 | 1              |
| 10     | Object Oriented Design  | 300               | 2020-8-8 13:03:12  | 4              |
| 12     | Dynamic Programming     | 2000              | 2018-8-18 20:03:12 | 1              |

在运行你的 SQL 语句之后，表应返回：

|          name           |
| :---------------------: |
|      System Design      |
|         Django          |
| Artificial Intelligence |
|        Java P6+         |

**样例二：**

表内容 : teachers

| id   | name             | email                                                        | age  | country |
| :--- | :--------------- | :----------------------------------------------------------- | :--- | :------ |
| 1    | Eastern Heretic  | [eastern.heretic@gmail.com](mailto:eastern.heretic@gmail.com) | 20   | UK      |
| 2    | Northern Beggar  | [northern.beggar@qq.com](mailto:northern.beggar@qq.com)      | 21   | CN      |
| 3    | Western Venom    | [western.venom@163.com](mailto:western.venom@163.com)        | 28   | USA     |
| 4    | Southern Emperor | [southern.emperor@qq.com](mailto:southern.emperor@qq.com)    | 21   | JP      |
| 5    | Linghu Chong     | NULL                                                         | 18   | CN      |

表内容：courses

| **id** | **name**               | **student_count** | **created_at**     | **teacher_id** |
| :----- | :--------------------- | :---------------- | :----------------- | :------------- |
| 1      | Senior Algorithm       | 880               | 2020-6-1 09:03:12  | 4              |
| 4      | Web                    | 340               | 2020-4-22 13:03:12 | 4              |
| 5      | Big Data               | 700               | 2020-9-11 16:03:12 | 1              |
| 8      | Data Analysis          | 500               | 2019-7-12 13:03:12 | 1              |
| 10     | Object Oriented Design | 300               | 2020-8-8 13:03:12  | 4              |
| 12     | Dynamic Programming    | 2000              | 2018-8-18 20:03:12 | 1              |

在运行你的 SQL 语句之后，表应返回：

name

> 因为 courses 表中没有 Western Venom 老师所教的课程，所以返回的课程名也为空，所以这里只展示了标题，没有数据。

一看就是多表查询题目，让其外键相等即可将两个表连接起来

题解：

```
select c.name
from courses as c , teachers as teachers
where c.teacher_id = teachers.id
and teachers.country = 'USA';
```

## 20.leetcode [第N高的薪水](https://leetcode.cn/problems/nth-highest-salary/)

表: `Employee`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
在 SQL 中，id 是该表的主键。
该表的每一行都包含有关员工工资的信息。
```

 

查询 `Employee` 表中第 `n` 高的工资。如果没有第 `n` 个最高工资，查询结果应该为 `null` 。

查询结果格式如下所示。

 

**示例 1:**

```
输入: 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
n = 2
输出: 
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```

**示例 2:**

```
输入: 
Employee 表:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
n = 2
输出: 
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| null                   |
+------------------------+
```

题解：

```
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT; 
    SET M = N-1; 
  RETURN (
     select distinct salary from employee order by salary desc limit m,1
  );
END
```

## 21.linkcode [我的最佳好友](https://www.lintcode.com/problem/3611/description?showListFe=true&page=1&problemTypeId=3&level=2&pageSize=50)

在本题中，存在以下三张表：

`Student` 表

| column | type |
| :----- | :--- |
| id     | int  |
| name   | int  |

`Friend` 表（最佳好友）

| column | type    |
| :----- | :------ |
| id     | int     |
| fid    | varchar |

`Score` 表

| column | type |
| :----- | :--- |
| id     | int  |
| value  | int  |

现在你需要根据这三张表，找到所有**最佳好友成绩比他们自己高**的学生，同时根据最佳好友的成绩进行降序排序，最后输出这些学生自己的名字。

最短时间刷“透”算法面试：《66页算法宝典》.pdf

微信添加【jiuzhangfeifei】备注【66】领取





多个人的最佳好友可能是同一个人。

一个人的最佳好友的最佳好友不一定是这个人自己。

样例

**样例 1**

输入：

`Student` 表：

| id   | name          |
| :--- | :------------ |
| 0    | Jack Rath     |
| 1    | Jackie Little |
| 2    | Sheila Hintz  |

`Friend` 表：

| id   | fid  |
| :--- | :--- |
| 0    | 1    |
| 1    | 2    |
| 2    | 0    |

`Score` 表：

| id   | value |
| :--- | :---- |
| 0    | 80    |
| 1    | 90    |
| 2    | 100   |

输出：

| name          |
| :------------ |
| Jackie Little |
| Jack Rath     |

**解释：**

- Jack Rath 的最佳好友 Jackie Little 成绩为 90，比 Jack Rath 自己的 80 高
- Jackie Little 的最佳好友 Sheila Hintz 成绩为 100，比 Jackie Little 自己的 90 高
- Sheila Hintz 的最佳好友 Jack Rath 成绩为 80，比 Sheila Hintz 自己的 100 低（因此最终结果不包含 Sheila Hintz）

由于 Jackie Little 的最佳好友成绩为 100，Jack Rath 的最佳好友为 90，因此按照最佳好友的降序顺序是 Jackie Little、Jack Rath。

**样例 2**

输入：

`Student` 表：

| id   | name          |
| :--- | :------------ |
| 0    | Jack Rath     |
| 1    | Jackie Little |
| 2    | Sheila Hintz  |

`Friend` 表：

| id   | fid  |
| :--- | :--- |
| 0    | 1    |
| 1    | 2    |
| 2    | 1    |

`Score` 表：

| id   | value |
| :--- | :---- |
| 0    | 100   |
| 1    | 90    |
| 2    | 80    |

输出：

| name          |
| :------------ |
| Jackie Little |

题解：

```
select s.name as name
from Student as s
join Friend as f on s.id = f.id 
join Score as sc1 on f.fid = sc1.id
join Score as sc2 on s.id = sc2.id
where sc2.value < sc1.value
order by sc1.value;
 
```

## 22.牛客网 [**SQL26** **计算25岁以上和以下的用户数量**](https://www.nowcoder.com/practice/30f9f470390a4a8a8dd3b8e1f8c7a9fa?tpId=199&tqId=1975678&ru=/exam/intelligent&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fintelligent%3Ftype%3Dvip)（if的使用）



题目：现在运营想要将用户划分为25岁以下和25岁及以上两个年龄段，分别查看这两个年龄段用户数量

**本题注意：age为null 也记为 25岁以下**

示例：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4    | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | male   | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

根据示例，你的查询应返回以下结果：

| age_cut    | number |
| ---------- | ------ |
| 25岁以下   | 4      |
| 25岁及以上 | 3      |

题解：

```
SELECT IF(age >= 25, '25岁及以上', '25岁以下') AS age_cut, COUNT(*) AS number

FROM user_profile

GROUP BY age_cut;
```

这里主要用了if查询

```
IF(condition, value_if_true, value_if_false)
```

- **condition**: 这是一个逻辑表达式，返回布尔值（`TRUE` 或 `FALSE`）。

- **value_if_true**: 当条件为 `TRUE` 时返回的值。

- **value_if_false**: 当条件为 `FALSE` 时返回的值。

解释

