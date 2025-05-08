---
title: Mysql 储存引擎
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.24
tags: 
   - mysql 

---

# 存储引擎

## mysql的体系结构

连接层：连接的问题，授权认证的功能等

服务层：最主要的功能级别都是在这进行的

引擎层：存储的引擎，可插拔式的引擎

存储层：存储的，系统文件和日志在这一层



## 存储引擎

默认的存储引擎

```
SELECT CREATE TABLE EMP;
```

RETURN:

```
可以看到默认的存储引擎 Inndb
```

创建表的时候指定存储引擎



```
CREATE TABLE NAME(
....



)ENGINE = INNODB;
```

查看当前数据库支持的存储引擎

```
show engines;
```

myisam早期版本的所使用的存储引擎

## 存储引擎介绍

### innodb

- dml操作遵循acid模型，支持事务

- 行级锁

- 支持外键

磁盘文件：

- xxx.idb这是一个表空间文件，存放了表结构数据和索引的文件
- 参数：innodb_file_per_table，每张表都对应一个表空间文件

这个文件是二进制的

可以使用指令打开

```
ibd2sdi xxxx.idb
```

打开文件里的数据

逻辑存储结构：

- 表空间
- 段
- 区
- 页
- 行

---进行sql优化使用

### myisam

不支持事务

支持锁表，不支持行锁

访问速度比较快

xxx.sdi	存储表结构信息

xxx.myd	存储数据

xxx.myl	存储索引

可以json格式化，然后查看文件

### memory

存放在内存中，作为缓存或者临时表

访问速度比较快，可以实现hash索引

xxx.sdi存储表结构

没有好坏之分，根据实际情况进行选择

**innodb：事务的完整性，数据的一致性比较高（（（（（绝大部分，这个是默认的）））））**

myisam：日志，服务器评论相关的

memory:常用来作缓存，储存在内存之中，无法保证数据的安全性
