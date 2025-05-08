---
title: Mysql sql优化
categories:
  - sql
abbrlink: 2701
date: 2024.7.31
tags: 

   - mysql 

---

# Sql优化

## 插入数据

批量插入：

```
insert into tb_name values(1,'a'),(2,'b'),(3,'c');
```

不要超过1k条，多的进入sql语句查询

手动提交：

```
start transaction;
insert into tb_name values(1,'a'),(2,'b'),(3,'c');
insert into tb_name values(1,'a'),(2,'b'),(3,'c');
insert into tb_name values(1,'a'),(2,'b'),(3,'c');
insert into tb_name values(1,'a'),(2,'b'),(3,'c');
commit;
```

所有的事务完成后，再提交数据

主键顺序插入：

```

```

大量数据插入使用load

```
先连接上mysql数据库
mysql --local-infile -u root -p
设置全局参数，开启本地文件加载
set global local_infile = 1;
执行load指令
load date local infile '/root/sql1.log' into table 'tb_user' fields terminated by ',' lines terminated by '\n';
```

sql数据在log日志文件里

## 主键优化

在innodb存储引擎中，表数据都是根据主键顺序组织存放的

一个区中包含64个页

页中存放的是具体的行数据，至少包含两行数据（数据过大，会行溢出）

顺序插入：

一个页满了，去写下一个页，他们之间会有一个双向指针

乱序插入：

会出现页分裂现象

页合并：

在删除一行数据的时候，不会真正的删除，只会记录标记

当记录达到merge_threshold（默认页的百分之五十）的时候，会找相邻的两个页是不是能够合并

主键的设计原则：

- 尽量降低主键的长度
- 插入数据时尽量选择顺序插入，选择使用auto_incerment
- 尽量不要使用uuid做主键或者其他自然主键，如身份证号

## order by优化

用using index的效率比较高，所以尽量用这个方式

backward index scan反向扫描索引，是desc进行排序的结果

创建索引的时候默认是升序排列，如果排序的时候用倒序，则会出现额外的排序

要创建新的索引

```
create index idx_user_age_pho_ad on ta_user(age asc,phone desc);
```

这样查询的时候age asc phone desc排序就不会出现filesort了

原则：

- 尽量使用覆盖索引
- 多字段排序的时候，要注意联合索引的升降序
- 如果出现filesort，大数据排序的时候，可是适当增大缓冲区的大小sort_buffer_size
- 要建立合适的索引，多字段排序的时候，也遵循最左前缀法则

## group by优化

索引对分组操作的影响：

尽量分组的时候要走索引的时候才能进行sql优化。

索引的使用也要满足最左前缀法则

## limit 优化

大数据下limit的优化

越往后性能越低，查询排序的条件非常大

在in之后不能使用limit语句

但可以使用多表联查，其中一个表为子查询。注意where条件和return数据

## count优化

count优化只能自己优化，大数据量的情况下比较耗时

count

是一个聚合函数，如果不是null则计数加一

count(*) count(主键) count(字段列表) count(null) count(1)

count(字段)：

- 没有not null约束的话，需要把字段值提取出来，在服务层进行判断，然后进行计数
- 有not null约束的话，会直接返回服务层，再进行累加

**sql性能：count(*)>count(1)>count(主键 id)>count(字段)**

## update优化

更新的字段没有索引，会锁住表

有索引的话，只会锁住行

行锁是对于索引进行加的锁，不要出现表锁，否则并发性能就会下降