---
title: Mysql面试题目
categories:
  - sql
abbrlink: 2708
date: 2025.3.15
tags: 

   - mysql 
---

# 基础

## 1.什么是mysql

MySQL 是一个**开源的关系型数据库**，现在隶属于 Oracle 公司。是我们国内使用频率最高的一种数据库，我在本地安装的是最新的 8.3 版本。

**怎么删除建立一张表？**

使用DROP TABLE 来删除表

CREATE TABLE来创建表

创建表的时候可以使用PRIMARY KEY来创建主键



```mysql
CREATE TABLE users (
    id INT AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    PRIMARY KEY (id)
);
```

**写一个升序/降序的sql语句**

```sql
SELECT id, name, salary
FROM employees
ORDER BY salary DESC;
```

```sql
SELECT id, name, salary
FROM employees
ORDER BY salary DESC, name ASC;
```

排序的第一依据是salary 降序，第二一依据是name 升序

**MYsql出现性能差的原因是什么？**

可能是sql查询的时候使用了全表扫描，也可能是查询语句过于复杂

也有可能是单表数据量过大。

我们可以添加索引来解决大部分问题，对于一些热点数据，可以使用Redis缓存

## 2.表的连接

内连接：

返回两个表中有**匹配关系**的行，找出两张表的**交集**

```sqlite
SELECT users.name, orders.order_id
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

INNER JOIN orders ON users.id = orders.user_id;连接条件：

users.id = orders.user_id

外连接：

不仅返回两个表中匹配的行，还返回没有匹配的行，用 `null` 来填充。

想当与找**并集**

left join右表没匹配的null填充

right join左表没匹配的null填充

```sql
SELECT users.id, users.name, orders.order_id
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

交叉连接：

返回两张表的**笛卡尔积**，也就是将左表的每一行与右表的每一行进行组合，返回的行数是两张表行数的乘积。

## 3.数据库的三大范式

第一范式：确保表的每一列都是**不可分割的基本数据单元**，比如说用户地址，应该拆分成省、市、区、详细地址等 4 个字段。

第二范式：要求表中的**每一列都和主键直接相关**。比如在订单表中，商品名称、单位、商品价格等字段应该拆分到商品表中。

第三范式：**非主键列应该只依赖于主键列**。比如说在设计订单信息表的时候，可以把客户名称、所属公司、联系方式等信息拆分到客户信息表中，然后在订单信息表中用客户编号进行关联。

建表的原则：

考虑表是否符合数据库的三大范式，确保字段不可再分，消除非主键依赖，确保字段仅依赖于主键等。

然后在选择字段类型时，应该尽量选择合适的数据类型。

在字符集上，尽量选择 utf8mb4，这样不仅可以支持中文和英文，还可以支持表情符号等。

当数据量较大时，比如上千万行数据，需要考虑分表。比如订单表，可以采用水平分表的方式来分散单表存储压力。

## 4.varchar和char的区别

varchar是可变长度的字符类型，原则上最多可以容纳 65535 个字符，但考虑字符集，以及 MySQL 需要 1 到 2 个字节来表示字符串长度，所以实际上最大可以设置到 65533。

varch中空格也要占一个字节

char是固定长度的字符类型，当定义一个 `CHAR(10)` 字段时，不管实际存储的字符长度是多少，都只会占用 10 个字符的空间。如果插入的数据小于 10 个字符，剩余的部分会用空格填充。

## 5.blob和text有什么区别

blob用于存储二进制数据，如图片音频等等，但是实际应用中，我们会把这些文件存储到oss或者文件服务器上，数据库存储文件的url

text用于存储文本数据，如文章评论等。

## 6.DATETIME和TIMESTAMP有什么区别

DATETIME直接存储日期和时间的完整值，与时区无关。

TIMESTAMP存储的是 Unix 时间戳，1970-01-01 00:00:01 UTC 以来的秒数，受**时区**影响。

DATETIME 的默认值为 **null**，占用 **8** 个字节；TIMESTAMP 的默认值为当前时间——**CURRENT_TIMESTAMP**，占 **4** 个字节，实际开发中更常用，因为可以自动更新。

## 7.in和exists的区别

使用 IN 时，MySQL 会首先执行子查询，然后将子查询的结果集用于外部查询的条件。这意味着**子查询的结果集需要全部加载到内存**中。

EXISTS 会对外部查询的每一行，执行一次子查询。如果子查询返回任何行，则 `EXISTS` 条件为真。`EXISTS` 关注的是**子查询是否返回行**，而不是返回的具体值。

```sql
-- IN 的临时表可能成为性能瓶颈
SELECT * FROM users 
WHERE id IN (SELECT user_id FROM orders WHERE amount > 100);

-- EXISTS 可以利用关联索引
SELECT * FROM users u
WHERE EXISTS (SELECT 1 FROM orders o 
            WHERE o.user_id = u.id AND o.amount > 100);
```

In适用于子查询结果集较小的情况。如果子查询返回大量数据，`IN` 的性能可能会下降，因为它需要将整个结果集加载到内存。

EXISTS适用于子查询结果集可能很大的情况。由于 `EXISTS` 只需要判断子查询是否返回行，而不需要加载整个结果集，因此在某些情况下性能更好，特别是当子查询可以使用索引时。

**NUll值陷？**

IN：如果子查询的结果集中包含 `NULL` 值，可能会导致意外的结果。例如，`WHERE column IN (subquery)`，如果 `subquery` 返回 `NULL`，则 `column IN (subquery)` 永远不会为真，除非 `column` 本身也为 `NULL`。

EXISTS：对 `NULL` 值的处理更加直接。`EXISTS` 只是检查子查询是否返回行，不关心行的具体值，因此不受 `NULL` 值的影响。

## 8.记录货币

货币在数据库中 MySQL 常用 **Decimal** 和 **Numeric** 类型表示，这两种类型被 MySQL 实现为同样的类型。他们被用于保存与货币有关的数据。

 salary DECIMAL(9,2)，9(precision)代表将被用于存储值的总的小数位数，而 2(scale)代表将被用于存储小数点后的位数。存储在 salary 列中的值的范围是从-9999999.99 到 9999999.99。

不使用 float 或者 double 的原因：因为 float 和 double 是以二进制存储的，所以有**一定的误差**。

会出现大问题的！

## 9.如何存储emoji

utf8 字符集仅支持最多 3 个字节的 UTF-8 字符，但是 emoji 表情（😊）是 4 个字节的 UTF-8 字符，所以在 MySQL 中存储 emoji 表情时，需要使用 utf8mb4 字符集。

```sql
ALTER TABLE mytable CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## 10.drop,delete,turncate的区别

都表示删除，但是三者有一些差别：

| 区别     | delete                                   | truncate                       | drop                                               |
| -------- | ---------------------------------------- | ------------------------------ | -------------------------------------------------- |
| 类型     | 属于 DML                                 | 属于 DDL                       | 属于 DDL                                           |
| 回滚     | 可回滚                                   | 不可回滚                       | 不可回滚                                           |
| 删除内容 | 表结构还在，删除表的全部或者一部分数据行 | 表结构还在，删除表中的所有数据 | 从数据库中删除表，所有数据行，索引和权限也会被删除 |
| 删除速度 | 删除速度慢，需要逐行删除                 | 删除速度快                     | 删除速度最快                                       |

因此，在不再需要一张表的时候，用 drop；在想删除部分数据行时候，用 delete；在保留表而删除所有数据的时候用 truncate。

## 11.UNION和UNION ALL的区别

- 如果使用 UNION，会在表链接后**筛选掉重复**的记录行
- 如果使用 UNION ALL，不会合并重复的记录行
- 从效率上说，UNION ALL 要比 UNION 快很多，如果合并**没有刻意要删除重复行，那么就使用 UNION All**

