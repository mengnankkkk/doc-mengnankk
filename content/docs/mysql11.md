---
title: Mysql 锁
categories:
  - sql
abbrlink: 2708
date: 2024.8.6
tags: 

   - mysql 

---

# 锁

## 概述

锁是计算机调解多个进程或者并发访问某一资源的机制

分类：

- 全局锁 锁住所有表
- 表级锁 锁住整张表
- 行级锁 锁住整个行的数据

## 全局锁

对整个数据库的实例进行加锁，整个实例处于只读状态

dml 和ddl语句都会阻塞

数据备份的时候会引发全局锁，从而获得一致性视图，来保证数据的完整性

加全局锁：

```
flush tables with read lock;//开锁
mysqldump -u -p itcast>itcast.sql;//备份
unlock tables;//关锁
```

在主库上备份，在备份期间都不能更新

在从库上备份，在备份期间不能执行从主库同步过来的二进制日志，会有主从延迟

```
mysqldump --single-transaction -uroot -p123 itcast > itcast.sql
```

这样就能在innnodb引擎上完成不加锁的一致性数据备份

## 表级锁

锁住整张表，锁定的颗粒大

分类---

- 表级锁
  - 表共享读锁 可以读取不能写入
  - 表独占写锁 其他的客户端不能写也不能读，但是当前客户端可以读也可以写
- 元数据锁
- 意向锁

表级：

加锁：

```
lock tables name....read/write
```

释放锁：

```
unlock tables
```

元数据锁：

在表上有活动事务的时候，不能对元数据进行写入操作。伪类避免dml和ddl冲突

对表增删改查的时候会自动加上mdl读锁（共享的）。对表结构进行变更的时候加mdl写锁（排他的）

**共享锁之间是兼容的，排他锁是不能够兼容的**

意向锁：

为了解决表锁和行锁的冲突，用意向锁来简化检查

意向共享锁和表锁共享锁是兼容的，与表锁排他锁互斥

```
select ...lock in share mode
```

意向排他锁和表锁的两种所互斥，意向锁之间不会互斥

```
insert update delete select...for update
```

查询意向锁和行锁的sql

```
SELECT * FROM information_schema.innodb_locks;
SHOW ENGINE INNODB STATUS;
```

## 行级锁

每次锁住的是数据行，锁定颗粒度最小，发生锁冲突的概率最低，并发度最高。

innodb的数据是根据索引组织的，行锁是通过索引上的索引项进行加锁的

分类

- 行锁，锁的是单行记录防止其他的update delete rr pr隔离级别都支持
- 间隙锁，锁定索引之间的间隙。防止其他事务对这个间隙insert，从而产生幻读。rr支持
- 临键锁，行锁和间隙锁组合，同时锁住数据和数据前面的间隙Gap,在rr隔离级别下支持

行锁：

共享锁：共享锁之间是兼容的

排他锁：允许排他锁的事务更新，组织其他数据获得相同的共享锁和排他锁

- insert update delete自动加排他锁

- select不加锁

- lock in share mode共享锁

- for update 排他锁

**在innodb中的行锁是根据索引加的锁，不通过索引检索数据，Inoodb将会升级为表锁**

间隙锁/临键锁：

在默认情况下innodb使用的是rr事务隔离级别，innodb会使用next-key锁进行搜索和扫描来防止幻读

**间隙锁是可以共存的**

- 唯一索引，给不存在的记录加锁时，优化为间隙锁
- 普通索引，向右遍历的最后一个指不满足查询条件，next-key退化为间隙锁
- 范围索引（唯一索引）会访问到不满足条件的第一个值为止

