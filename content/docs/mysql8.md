---
title: Mysql 索引
categories:
  - sql
abbrlink: 2701
date: 2024.7.25
tags: 

   - mysql 

---

# MySQL 索引

## 介绍

是帮助mysql高效的获取数据的数据结构（有序的）来指向原始数据，可以在这些数据结构上实现高级查找算法。

没有索引：

是按照上下顺序来直接查找的，会匹配整张表---叫做全表扫描

二叉树演示，会根据指向找到这条记录。，有索引的情况下搜索效率比较高

优缺点：提高排序效率，提高检索效率

会占用磁盘空间，但降低了表更新的速度

## 结构

mysql的索引是在引擎层实现的

b+tree索引	最常见的	都支持

hash索引	精确匹配	memory支持

r-tree索引	空间位置	myisam支持

full-text索引	除了memory不支持其他都支持

b-tree多路平衡查找树

几阶b-tree就最多有几个节点，会有n+1个指针

### 构建b-tree

**满了中间元素向上分裂**每一个key都会对应



### 构建b+tree

在b+树中所有的元素都会在叶子节点，叶子节点会形成一个单向链表，非叶子节点只会起到索引的作用

mysql的所有，会形成了一个带有顺序的指针b+tree

所有的数据都会在叶子节点，用来存储数据，存储在页中

### hash索引

先算出每一行的hash值，将键值换成新的hash值，映射到对应的槽位上，存储的hash表中

多个key所以映射到同一个槽位中，会出现hash碰撞，可以通过链表解决

只能用于= in不支持范围

无法排序

效率比较高，除非出现hash碰撞

**为什么innodb选择B+tree？**

- 对于b+树对于二叉树的层级更少，搜索效率高

- 相对于b-ree无论是叶子节点还是非叶子节点，都会保存数据，会导致键值变少，指针变少，同样要保留大量数据，只能增加树的高度，导致性能降低
- 对于hash索引来说，可以范围索引和排序

## 索引的分类

主键索引，对于主键的创建的索引，只能有一个，默认自动创建 primary

唯一索引，避免同一个表中的数据列的值重复，unique

常规索引 快速定位数据

全文索引 查找文本中的关键字，而不是索引中的值，关键词fulltext

还分为

聚集索引：数据存储和索引放在一个，必须有而且**只能有一个**

二级索引，将数据和索引分开，叶子节点关联的是对应的主键

聚集索引：

- 存在主键，主键索引就是聚集索引
- 不存在主键，将会引用第一个唯一索引作为聚集索引
- 没有主键，没有唯一索引，Innodb会自动生成一个rowid作为隐藏的聚集索引

回表查询：先走二级索引找到对应的主键值，再到聚集索引中拿到这一行的行数据

思考：

innodb的b+tree高度是多少？

16*1171^h,高度为h

## 索引的操作语法

创建索引：

```
CREATE [UNIQUE/FULLLTEXT] INDEX INDEX_NAME ON TABLE_NAME(INDEX_COL_NAME...);
```

一个索引是可以关联多个字段的

查看索引：

```
show index from table_name;
```

删除索引：

```
drop index index_name on table_name;
```

索引名字格式

```
idx_user_name;
```

# sql性能优化

## sql的执行频率

看当前的数据库的增删改为主优化不用太好，查的频率高要进行优化

```
select global status like 'com_______';
```

查看当前的执行频率

在这里面进行查看

### 慢查询日志

查看超过指定时间（默认10s）的sql语句

```
show variables like 'long_query_time';

```

查看状态

在/etc/my.cnf

设置其中的两个参数

```
slow_query_log=1
long_query_time=2
```

这样就能开启了，超过两秒的就会被计入慢查询日志中

### profile

```
show profiles;
```

了解时间耗时情况

默认这个开关时关闭的

```
set profiling = 1;
```

开启开关

```
show profile cup for query 16;
```

cup的耗时情况

### explain

可以看到sql语句的执行情况

```
explain +sql语句；
```

- id:select查询的序列号，执行顺序从上到下，id不同，值越大越先执行。展示表的执行顺序
- select_type：表示select的类型，simple(简单表，不用表连接和子查询) primmary（主查询，即外层的查询）

union（union中的第二个或者后面的查询语句） subquery(select/where之后包含了子查询)等

- type：表示连接类型，性能好到差的null system const eq_ref ref range index all
- possible_key：可能用到的索引
- key：实际用到的索引
- ken_len：索引中的长度
- row：执行查询的行数，只是一个预估值
- filerted:返回结果的行数站所要读取的行数的百分比，越大越好



## 索引的使用

原则：

### 最左前缀法则

查询使用索引要从索引的最左列开始，不能跳过，跳过的话，后面的索引就会失效

必须包含最左边的一列，放在哪都行，必须要存在

### 范围查询

联合索引中，出现范围查询(<>)范围查询右侧的索引列失效

使用大于等于或者小于等于就可以避免后面的索引失效

### 索引列运算操作

不用在索引列上进行运算操作，否则索引会失效





**不能不加单引号，否则索引也会失效**

### 模糊查询

尾部进行模糊匹配，索引不会失效。头部进行模糊匹配索引会失效

后面加%可以进行索引，前面加%索引就会失效。一定要规避这样的sql语句



## 索引的使用原则

### or的连接条件

or的两侧都有索引，索引才会成功，一侧没有索引，索引就会失效

### 数据分布影响

使用索引比全表扫面还慢的话，就不要用索引了

### sql提示

优化数据库的手段

提示：

- use index 建议

```sql
explain select * from tb_user use index(idx_user_pro) where profession = 'ruangong';
```

- ignore index  忽略

  ```sql
  explain select * from tb_user ignore index(idx_user_pro) where profession = 'ruangong';
  ```

- force index  强制

  ```
  explain select * from tb_user force index(idx_user_pro) where profession = 'ruangong';
  ```




### 覆盖索引

尽量使用覆盖索引(查询使用了索引，并且需要返回的列，再改索引中全部都能找到)

查询的字段要在联合索引里面，否则就会出现查询使用了索引，但是还需要进行回表查询

### 前缀索引

当字段列表是字符串的时候，要引用的字符串很大，索引变得很大，浪费大量的io

只将字符串的一部分前缀建立索引

```sql
create index idx_xxx on tablle_name(column(n));
```

求取选择性：

```
select count(distict email)/count(*) from table_name;
```

选择性要大，而且前缀不要太多

建立索引：

```sql
create index idx_email_5 on table(email(5));
```

### 单列索引和联合索引的选择

推荐使用联合索引，建立联合索引进行查询。

## 索引原则

- 数据量大，查询比较频繁的表要建立索引，对查询条件进行索引，尽量使用联合索引。

- 要使用区分度高的索引

- 字符串类型的索引，要建立前缀索引。要考虑前缀的区分度
- 要控制索引的效率
- 索引不能存储null值，建立表的时候要采用not null的约束
