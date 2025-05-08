---
title: Mysql 事务
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.22
tags: 
   - mysql 

---

# 事务

简介：

是一系列操作的集合，是一个不可分割的工作单位（原子性）

要么同时成功要么同时失败

- 开始事务
- 错误了就回滚事务
- 然后提交事务

实例银行：

查询账户余额：

```
SELECT & FROM ACCOUNT WHERE NAME = "A";
```

将账户余额-1000

```
UPDATE ACCOUNT SET MONEY = MONEY -1000 WHERE NAME = 'A';
```

别人账户余额+1000

```
UPDATE ACCOUNT SET MONEY = MONEY + 1000 WHERE NAME = 'B';
```



## 事务控制

```
SELECT @@AUTOCOMMIT;
SET @@AUTOCOMIIT = 0;
```

设置为手动提交

```
commit;
```

提交

```
rollback;
```

回滚事务

开启事务

```
START TRANSACTION 或 BEGIN;
```

## 事务的四大特性

原子性：事务是不可分割的最小操作元，要么全部成功，要么全部失败

一致性：事务完成时，必须所有数据必须保持一致状态

隔离性：数据库提供隔离机制，事务不受外部并发环境的影响

持久性：事务一旦提交或者回滚看，在数据库中的数据改变就是永久的

## 事务并发问题

### 脏读

一个事务读到另一个事务还没提交的数据

### 不可重复读

一个事务先后读取到同一条记录，但是两次读取的数据不同

### 幻读

一个事务按照条件查询数据的时候，没有对应的数据行，但是在插入数据的时候，发现这行数据已经存在了，好像出现了幻觉

## 事务的隔离级别

解决事务的问题

read uncommitted 	三种问题都会出现

read comitted	解决了脏读的问题

repeatable read（mysql默认）	无法解决幻读的问题

serializable(看情况)	三种问题都能解决

```
SELECT @@TRANSACTION_ISOLATION
```

查看当前的隔离级别

```
SET SESSION TRANSACTION_ISOLATION LEVEL [几种级别]
```

设置当前的隔离级别
