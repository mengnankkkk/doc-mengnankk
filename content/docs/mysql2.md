---
title: Mysql DQL
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.8
tags: 
   - mysql 

---

# DQL(数据库的查询)

## 查询语法

查询关键词：SELECT

语言结构：（编写顺序）

```
SELECT 字段列表
FROM 表名查询
WHERE 条件列表
GROUP BY 分组字段列表
HAVING 分组后条件列表
ORDER BY 排序字段列表
LIMIT 分页参数

```

## 基础查询

1.查询多个字段

```
SELECT 字段列表(1),字段列表(2)...FRIM TABLE_NAME; 某个字段查询

SELECT * FROM TABLE_NAME; 全字段查询#实际开放中不要使用
```

2.设置别名

```
SELECT 字段列表  '别名' FROM TABLE_NAME;
```

3.去除重复记录

```
SELECT DISTINCE 字段列表 '别名' FROM TABLE_NAME;
```

## 条件查询

1.语法

```
SELECT 字段列表 FROM WHERE 条件列表;
```

下表中实例假定 A 为 10, B 为 20

| 操作符 | 描述                                                         | 实例                 |
| :----- | :----------------------------------------------------------- | :------------------- |
| =      | 等号，检测两个值是否相等，如果相等返回true                   | (A = B) 返回false。  |
| <>, != | 不等于，检测两个值是否相等，如果不相等返回true               | (A != B) 返回 true。 |
| >      | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true | (A > B) 返回false。  |
| <      | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true | (A < B) 返回 true。  |
| >=     | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true | (A >= B) 返回false。 |
| <=     | 小于等于号，检测左边的值是否小于或等于右边的值, 如果左边的值小于或等于右边的值返回true | (A <= B) 返回 true。 |

条件还可以包括逻辑运算

AND &&#和

OR ||#或

NOT ! #非

2.

判断NULL使用IS NULL

```
SELECT * FROM TABLE_NAME WHERE 字段列表 IS NULL;
```

```
SELECT * FROM TABLE_NAME WHERE 字段列表 IS NOT NULL;
```

3.BETWEEN AND 

```
SELECT * FROM TABLE_NAME WHERE 字段列表 BETWEEN A AND B;#B>A
```



4.IN

```mysql
SELECT * FROM TABLE_NAME WHERE 字段列表 IN(A,B,C);
```

5.LIKE

(_匹配单个字符，%匹配任意个字符)

通配符 `%` 的作用是匹配零个或多个字符。例如：

- **`'j%'`：匹配以 'j' 开头的所有字符串。**
- **`'%j'`：匹配以 'j' 结尾的所有字符串。**
- **`'%j%'`：匹配包含 'j' 的所有字符串。**

```
SELECT * FROM TABLE_NAME WHERE 字段列表 LIKE '__';
```

查询字段列表为两个的数据

## 聚合函数

介绍：**一列数据作为一个整体，进行纵向计算**

```
COUNT #统计数量
MAX 
MIN
AVG #平均值
SUM #求和
```

```
SELECT COUNT(字段列表) FROM TABLE_NAME;#NULL值不参与计算;
```

**#NULL值不参与计算**

## 分组查询

```
SELECT 字段列表 FROM TABLE_NAME WHERE 条件 GROUP BY HAVING 分组后过滤的条件;
```

- **WHERE 条件分组前过滤，HAVING 分组后进行过滤**

- **WHERE 不能对聚合函数进行判断 HAVING 可以**
- **执行顺序：WHERE >聚合函数>HAVING**
- **分组后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义**

实例：

```
SELECT GENDER,COUNT(*) FROM EMP GROUP BY GENDER;
```

```
SELLECT WORKADDRESS,COUNT(*) FROM EMP WHERE AGE < 45 GROUP BY WORKADDRESS HAVING COUNT(*) >= 3;
```

## 排序查询

```
SELLECT 字段列表 FROM TABLE_NAME ORDER BY 字段1 排序方式1,字段2 排序方式2;
```

- ASC(升序)（默认）
- DESC（降序）

- 如果是多字段排序，当第一个字段的VALUE相同的时候，才会根据第二个字段排序

实例：

```
SELECT * FROM EMP ORDER BY AGE ASC;
```

ASC可省略（默认）

```
SELECT * FORM EMP ORDER BY AGE ASC,TIME DESC;
```

## 分页查询

```
SELECT 字段列表 FROM TABLE_NAME LIMIT 起始索引,查询的记录数;
```

- 起始索引从0开始，起始索引= **（查询页码-1）*每页显示记录数**
- 分页查询是数据库的方言，不同的数据库有不同的实现，MYSQL中是LIMIT
- 如果查询的是 第一页数据，起始索引可以省略，简写为LIMIT 10；

实例：

```
SELECT * FORM EMP LIMIT 0,10;
```

```
SELECT * FORM EMP LIMIT 10,10;
```

实例：

```
SELECT * FROM EMP WHERE GENDER = 'NV' AND AGE IN (20,21,22,23);
```

```
SELECT * FORM EMP WHERE GENDER = 'NAN' ADN BETWEEN 20 AND 40 AND NAME LIKE '___';
```

```
SELECT GENDER, COUNT(*) FORM EMP WHERE AGE <60 GROUP BY GENGDER;
```

```
SECLECT NAME , AGE FORM EMP WHERE AGE <= 35 ORDER BY AGE ASC , TIME DESC;
```

```
SELECT * FORM EMP WHERE AGE BETWEEN 20 AND 40 WHERE GENDER = 'NAN' ORDER BY AGE ASC , TIME DESC LIMIT 5;
```

## DQL执行顺序

执行顺序FROM→  WHERE→ GROUP BY →SELECT →ORDER BY →LIMIT 

## 小结

语言编写顺序：

```
SELECT 字段列表
FROM 表名查询
WHERE 条件列表
GROUP BY 分组字段列表
HAVING 分组后条件列表
ORDER BY 排序字段列表
LIMIT 分页参数
```

# DQL多表查询
