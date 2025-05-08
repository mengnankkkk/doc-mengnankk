---
title: Mysql 函数
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.11
tags: 
   - mysql 

---

# MYSQL函数&&约束

## 函数

函数是指一段可以直接被另一程序调用的程序或者代码

### 字符串函数

CONCAT(S1,S2...)将字符串拼接，S1,S2...拼接成一个字符串

LOWER(STR) 	将STR全部转变为小写

UPPER(STR)	将STR全部转变为大写

LPAD(STR,N,PAD)	左填充，用字符串PAD对STR的左边进行填充，达到n个字符串的长度

RPAD(STR,N,PAD)	右填充，用字符串PAD对STR的右边进行填充，达到n个字符串的长度

TRIM(STR)	去掉字符串头部和尾部的空格

SUBSTRING(STR,START,LEN)	返回从字符串str从start位置起的len个长度的字符串

```mysql
SELECT CONCAT('HELLO','MYSQL');
#HELLO MYSQL
```

```mysql
SELECT LPAD('01',5,'-');	#---01
SELECT RPAD('01',5,'-');	#01---
```

```mysql
SELECT TIRM(' HELLO MYSQL ');
#HELLO MYSQL
```

```mysql
SELECT SUBSTRING('HELLO MYSQL',1,5);
这个函数是从1开始的
#HELLO
```

```
UPDATE EMP SELECT WORKNO = LPAD('1',5,'0');	#00001
```



### 数值函数

CEIL(X) 	向上取值

FLOOR(X）向下取值

MOD(X,Y)	返回x/y的模

REND()	返回0~1的随机数

ROUND(X,Y)	求参数x的四舍五入值，保留y位小数

```
SELECT CEIL(1.5);	#2
```

```
SELECT LPAD(ROUND(REND()*1000000,0),6,'0');
```



### 日期函数

CURDATE()返回当前的日期

CURTIME()返回当前的时间

NOW()返回当前的日期和时间

YEAR(DATE)获取指定的年份

MONTH(DATE)获取指定的月份

DAY(DATE)获取指定的日期

DATE_ADD(DATE,INTERVAL EXPR TYPE)返回一个日期/时间值，加上一个时间间隔expr后的时间值

```
SELECT DATE_ADD(NOW(),INTERVAL 70 YEAR);
```

DATEDIFF(DATE1,DATE2)返回起始时间date1和结束时间date2之间的天数

```mysql
SELECT NAME, DATEDIFF(CURDATE(), DATE) AS TIANSHU
FROM EMP
ORDER BY TIANSHU DESC;

```



### 流程函数

IF (VALUE,T,F)	如果value为true则返回t，否则返回f

IFNULL(VALUE1,VALUE2)	如果value1不为空则返回value1否则则返回value2

CASE WHEN [VAL1] THEN [RES1]....ELSE [DEFAULT] END 	如果val1为true，则返回res1。。否则则返回default默认值

CASE【expr】 WHEN [VAL1] THEN [RES1]....ELSE [DEFAULT] END 	如果expr的值为val1，则返回res1。。否则则返回default默认值

```mysql
SELECT NAME,
(CASE WORK WHEN 'BEIJING' THEN '1' WHRN 'SHANGHAI' THEN '1' ELSE '2') AS 'ADDRESS'
FROM EMP;
```

```mysql
SELECT NAME,
(CASE WHEN MATH >= 85 THEN 'A' WEHN MATH >=60 THEN 'B' ELSE 'C' AND) '数学',
FROM EMP;
```

## 约束

概述：作用于表中字段上的规则，限制储存在表中的数据。

非空约束 	限制该字段的数据不能为null   	**not null**

唯一约束 	保证该字段的所有数据都是唯一的 	**unique**

主键约束 	主键是一行数据的所有数据，要求非空且唯一 	**primary key**

默认约束 	保存数据时未指定该数据，采用默认值 	**default**

检查约束 	保证该字段满足某一个条件 	**check**

外键约束	 两张表的数据建立联系，保证数据的一致性和完整性	 **foreign key**

实例:一个字段可添加多个约束

```
primary key,auto_incerment#主键 自动增长
```

```mysql
CREATE TABLE user (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '主键',
    name VARCHAR(10) NOT NULL UNIQUE COMMENT '姓名',
    age INT CHECK (age > 0 AND age <= 120) COMMENT '年龄',
    status CHAR(1) DEFAULT '1' COMMENT '状态',
    gender CHAR(1) COMMENT '性别'
);

```

```mysql
inserrt into user(name,age,status,gender) values ('tom1','19','1','nan');
```

外键约束

具有外键的叫做子表，没有的叫作父表。这是一个逻辑关系

保证数据的一致性和完整性

```
create table table_name()
```

```
alter table emp add_constraint fk_emp_dept_id foregin key (dept_id) references dept(id);
```

- emp表名

- fk是foregin key缩写

- dept_id表的列

```
alter table emp drop foregin key fk_emp_dept_id;
```

外键的约束删除和更新

no action	父表中删除更新时，是否有对应的外键，如果有则不允许更新删除

restrict	父表中删除更新时，是否有对应的外键，如果有则不允许更新删除

cascade	父表和子表对应的字段同步

set null	父表进行更新或者删除的时候，是否有对应的外键，有则设置子表对应的外键为null

set default	
