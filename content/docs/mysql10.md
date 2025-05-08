---
title: Mysql 存储对象
categories:
  - sql
abbrlink: 2708
date: 2024.8.1
tags: 

   - mysql 

---

# Mysql 存储对象

## 视图

### 介绍

是一种虚拟存在的表，视图中的数据在数据库中不存在。行和列数据来自自定义视图查询使用的表，视图不保存数据，只是保存了sql的逻辑。

```
create [or replace] view 视图名称{（列表名称）} as select语句 []
```

演示：

```
create or replace view stu_v_1 as select id,name from student where id <= 10;
```

创建了一个视图

查询视图：

```
show create view 视图名称
select from 视图名称....是一个虚拟存在的表，可以像表一样进行操作。
```

修改视图：

```
修改和创建视图的语法是一样的
alter view 视图名称【（视图列表）】 as select语句；
```

删除视图：

```
drop view [if exists] 视图名称 [视图名称];
```

### 视图操作和检查选项

在select语句后面加上with casecaded check option加上一个检查选项.就可以避免后来又插入了一些数据，，一些插入到数据不属于selcet语句的要求。

会通过视图检查正在更改的每个行，mysql允许基于一个视图去关联另一个视图

默认值为casecaded 还有一个值为local

local：

使用local只会看当前是不是满足，然后会递归去寻找他的源视图的条件，如果没有检查选项，就不进行条件的检查，有检查选项就会进行检查

casecaded：

使用casecaded的时候会看当前的是不是满足，然后会递归去寻找他源视图的条件。相当于源视图里也加上了一个with casecaded check option

### 视图-更新

视图更新的条件：

- 视图中的行必须和基础表中的行存在一一对应的关系，如果包含以下的任意一种情况，则该视图不可更新
- 聚合函数或者窗口函数(sum() max() min() count())
- distinct
- group by
- having
- union 或者 union all

### 视图的作用

- 简化操作

- 安全
- 数据独立

### 案例

创建一个视图，只能看见要屏蔽某某字段

```sql
create view tab_user_name as select id,name,...from tb_user;
```

查询每个学生的课程（三表联查）

```sql
selcet * from student s,student_course sc,course c where s.id = sc.studentid and sc.id =  c.id;
```

先写出三张表的联查sql语句

```sql
create view tab_stu_course_view as selcet s.name studentname,s.no student_no,c.name course_name from student s,student_course sc,course c where s.id = sc.studentid and sc.id =  c.id;
```

然后直接查询这个视图

```sql
select * from tab_stu_course_view;
```

## 存储过程

### 介绍

是事先通过编译并存储在数据库中一段sql语句的集合，可以简化工作，减少数据在数据库和应用服务器之间的传输

就是sql语句语言层面的代码封装和重用

特点：

- 封装，重用
- 可以接受参数，也可以返回数据
- 减少网络交互，提升效率

创建：

```
create procedure 存储过程名称（【参数列表】）
begin
sql语句
end;
```

调用：

```
call 名称([参数])；
```

查看：

```
show * from information_schema.routines where routine_schema = 'xxxx'查询xxxx数据库的存储过程和状态
show create procedure 存储过程名称；查询某个存储过程的定义
```

删除

```
drop procedure [if exists] 存储过程名称；
```

在命令行中运行会有小bug

当在命令行中运行的时候，看到分号说明语句已经结束了

在命令行中需要使用delimiter来指定结束($$)

```sql
 delimiter $$
 create procedure 存储过程名称（【参数列表】）beginsql语句end$$
```

结束符变成了$$

### 变量

系统变量：

mysql中服务器自己提供的变量

全局变量和会话变量

全局的是所有的都会使用，会话变量只是在当前会话应用的变量

```
show [session|global].variables;查看所有系统变量
show [session|global].variables like '';模糊匹配
show @@[session|global].系统变量名；查看指定的变量
```

设置系统变量

```
set [session|global].系统变量名= 值；
set @@[session|global].系统变量 = 值；
```

不指定默认是session

用户自定义变量：

不需要提前声明

@变量名 就行了，只能在当前会话使用

```
set @var_name = expr [,@var_name = expr];
set @var_name ：= expr [,@var_name:= expr];
```

还可以执行select语句

```
select @var_name := expr [,@var_name := expr];
select 字段名 into @var_name from tab_name;
```

使用：

```
select @var_name;
```

=即可以当作赋值也可以比较 ：=做为赋值运算符

用户变量无需声明和初始化，没有赋值则作null值

局部变量：

在begin 和 end块里

声明：

```
declare 变量名 变量类型【default...]
```

变量类型就是数据库字段的类型，例如int啥的

赋值：

```
set 变量名 = 值；
set 变量名 := 值；
select 字段名 into 变量名 from tab_name...;
```

使用：

```
create procedure p1()
begin
	declare stu_count int default 0;
	select count(*) into stu_count from sudents;
	select stu_count;
end
call p1();
```

### 条件判断

```
if 条件1 then
elseif 条件2 then
....
else....
end if;来结束
```

还可以使用case

```
CASE expression
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ...
    ELSE default_result
END case;

```

如果表达式成立，则进行

也可以不加条件判断，直接进行匹配

### 参数

in输入参数	默认的

out输出参数

inout既可以输入，也可以输出

```
create procedure name(in/inout 参数名 参数类型)
begin
...
end;
```

例子：

```
create procedure level(in score int,out result varchar(10))
begin
if score >=80 then
	set result := 'hao';
	....
else
	..
end if;
end;


call level(68,@result);
select @result;
```

inout:

```sql
create procedure return(inout score double)
begin
set socore := score * 0.5;
end;


set @score = 78;
call return(@scoure);

```

concat函数，传递字符串

```
select concat ('aaaaa'变量....);
```

### 循环

#### while

```
while 条件 do 
sql...
end while;
```

实例：

```sql
create procedure p(in n int)
begin 
declare total int default 0;
whlie n>0 do 
set total := total + n;
set n := n - 1;
end while;
select total;
end;
```

实现一个累加

#### repeat

满足条件的时候退出循环

先执行一次逻辑，看条件满足不满足

然后进行下面的操作

```
repeat
sql....
until 条件
end repeat;
```

#### loop

可以实现死循环

leave 退出循环

iterate在循环中使用,进入下一次循环

```
[begin_label:] loop
sql..
end loop;[end_label];
```

前后两个是标记

实例：

```sql
CREATE PROCEDURE p(IN n INT)
BEGIN
    DECLARE total INT DEFAULT 0;
    
    sum_loop: LOOP
        IF n <= 0 THEN
            LEAVE sum_loop;
        END IF;
        
        SET total := total + n;
        SET n := n - 1;
    END LOOP sum_loop;
    
    SELECT total;
END;

```

### 游标

存储查询结果集的数据类型

```
declare 游标名称 cursor for 查询语句；
```

打开游标

```
open 游标名称
```

获取游标记录

```
fetch 游标名称 into 变量；
```

关闭游标

```
close 游标名称；
```

实例：

```sql
create procedure p(in uage int)
begin
declare uname varchar(100);
declare upro varchar(100);
declare u_cursor cursor for select name,pro from tb_user where age<=uage;
declare exit hander for SQLSTATE '02000' close u_cursor;
create table if not exists tb_user_pro(
	id int primary key auto _increment.
	name varchar(100),
	pro varchar(100)
);
open u_cuisor;
whlie true do
fetch u_user into uname ,upro
insert into tb_user_pro values(null,uname,upro);
end while;
close u_cursor;
end;
call p(40);
```

SQLSTATE '状态码'

SQL WARNING 以01开头所有状态码简写

NOT FOUND 02开头的所有状态码简写

SQLEXCEPTION 所有没有被前面两个状态码的简写

### 条件处理程序

```
DECLARE hanller_action HANDLER FOR condition_value .....sql...;
```

- handler_action:

  - continue继续执行
  - exit终止

- condittion_value:

  

  - SQLSTATE '状态码'
  - SQL WARNING 以01开头所有状态码简写
  - NOT FOUND 02开头的所有状态码简写
  - SQLEXCEPTION 所有没有被前面两个状态码的简写

  实例：

  在遇到0200的时候会关闭游标，然后退出程序

```
declare exit handler for SQLSTATE '0200' CLOSE u_USERS;
```

### 存储函数

是有返回值的存储过程，必须是类型的

```
create function test()
returns type [characteristic]
begin
sql....
reurn...
end;
```

其中characteristic:

- DETERMINISTIC 相同的参数总是产生相同的结果
- no sql 不包含sql语句
- reads sql data 包含读取数据的语句，但不包含写入数据的语句

从1累加到n的存储函数

```
create function fun(n int)
returns int deterministic 
begin
declare total int default 0;
        while n >0 do
              set total :=total + n;
              set n := n-1;
        end while;
        return total;
end;
select fun(100);
```

## 触发器

与数据库表有关的数据库对象，在删除插入更新数据的之前之后都可以触发触发器

就可以在数据库端进行保存数据的完整性日志数据校验等工作

old和new来引用触发器

- insert ---new
- update--old--new
- delete---old

现在只支持行级触发器，不支持语句触发器。

创建触发器：

```
create trigger name
    before /after insert/update/delete
    on tab_name for each row//行级触发器
    begin
    sql...
    end;
```

查看触发器

```
show triggers;
```

删除触发器

```
drop tigger [date_name]tigger_name;
```

实例：

```
create trigger ta_user_insert_tigger
    after insert on tb_user for each row 
    begin 
        insert into user_logs(..............) values;
        (null,'insert',now(),new.id,concat('插入的数据内容为'+id+'name='+new.name));
    end;
```

小结

视图是一个虚拟存在的表，不保存查询结果。数据独立，数据安全。
