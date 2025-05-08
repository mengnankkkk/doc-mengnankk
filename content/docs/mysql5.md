---

title: Mysql 多表查询
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.19
tags: 
   - mysql 

---

# 多表查询

## 关系：

### 一对多

例如：部门和员工之间的关系

实现：在多的一方建立外键，指向一的一方

### 多对多

例如：学生和课程的关系

实现：建立第三张中间表，至少包含两个外键，分别关联两方

### 一对一

例如：用户与用户详情之间的关系

实现：在任意的一方加入外键，关联另一方的主键，并且设置外键是唯一的

```sql
CREATE TABLE IF NOT EXISTS tb_user (
                                       id INT AUTO_INCREMENT PRIMARY KEY COMMENT '用户ID',
                                       name VARCHAR(10) COMMENT '姓名',
                                       age INT COMMENT '年龄',
                                       gender CHAR(1) COMMENT '性别，1：男，2：女',
                                       phone CHAR(11) COMMENT '手机号',
                                       CONSTRAINT uc_phone UNIQUE (phone) -- 添加唯一约束，确保手机号唯一
) COMMENT '用户基本信息表';

-- 创建用户教育信息表 tb_useredu
CREATE TABLE IF NOT EXISTS tb_useredu (
                                          id INT AUTO_INCREMENT PRIMARY KEY COMMENT '教育信息ID',
                                          degree VARCHAR(20) COMMENT '学历',
                                          major VARCHAR(50) COMMENT '专业',
                                          primaryschool VARCHAR(50) COMMENT '小学',
                                          middleschool VARCHAR(50) COMMENT '中学',
                                          university VARCHAR(50) COMMENT '大学',
                                          userid INT COMMENT '用户ID',
                                          CONSTRAINT fk_userid FOREIGN KEY (userid) REFERENCES tb_user(id) ON DELETE CASCADE ON UPDATE CASCADE
) COMMENT '用户教育信息表';
```

**CONSTRAINT uc_phone UNIQUE (phone) -- 添加唯一约束，确保手机号唯一	主要是这个用来确定1对1**

## 多表查询

单表查询

```
SELECT * FORM EMP;
```

多表查询

```
SELECT * FORM EMP , DEPT;
```

笛卡尔积：a集合和b集合所有的组合情况

直接查询的是表的笛卡尔积

所以要消除无效的笛卡尔积，要加入条件

```
SELECT * FORM EMP , DEPT WHERE EMP.DEPT_ID = DEPT.ID;
```

这样就能找到其有效的

## 分类

### 内连接

两张表之间交集的部分

隐式内连接

```
SELECT 字段列表 FROM TABLE_1 , TABLE_2 WHERE....;
```

显示内连接

```
SELECT 字段列表 FROM TABLE_1 [INNER]JOIN TABLE_2 ON 连接条件....；
```



### 外连接

```
SELECT 字段列表 FROM TABLE_1 LEFT [OUTER] JOIN TABLE_2 ON WHERE...;
```

相当与查询表1（左表）的所有数据包含表1和表2交集的部分

```
SELECT 字段列表 FROM TABLE_1 RIGHT [OUTER] JOIN TABLE_2 ON WHERE...;
```

相当与查询表2（右表）的所有数据包含表1和表2交集的部分



### 自连接

```
SELECT 字段列表 FROM TABLE_A A JOIN TABLE_A B ON WHERE（条件）;
```

实例：

```
SELECT A.NAME B.NAMME FORM EMP A , EMP B WHERE A.MAID = B.ID;
```

### 联合查询

就是把多次查询的结果合并起来

```
SELECT 字段列表 FROM A....;
UNION[ALL]
SELECT 字段列表 FROM B....;
```

多张表的列数必须保持一致，字段类型也需要一致。

### 子查询

  在sql语句中嵌套select语句

```
SELECT *FROM T1 WHERE COLUMN1 = (SELECT COLUMN1 FORM T2);
```

外部的语句可以是insert update delete select其中的任意一个

- 标量子查询(子查询的结果为单个值)

- 列子查询（子查询的结果为单个值）
- 行子查询（子查询的结果为一列）
- 表子查询（子查询的结果为多行多列）

子查询的位置

- where 之后
- from之后
- select之后

#### 标量子查询

常用的操作符号 = <> > =....

```
select * from emp where dept_id = (select id from dept where name = 'aaa');
```

#### 列子查询

常用用操作符：

- in
- not in
- any（返回的列表中，任意一个就行）
- some
- all（所有的指都要满足）

```
SELECT ID FROM DEPT WHERE NAME = 'AA' OR NAME = 'BB';
```

```
SELECT * FROM EMP WHERE DEPT_ID IN (SELECT ID FROM DEPT WHERE NAME = 'AA' OR NAME = 'BB');
```

#### 行子查询

**= <> in not in**等操作符

主要是分布查询，然后用子查询来完成任务



#### 表子查询

```sql
SELECT * FROM EMP WHERE (JOB,SALARY) IN (SELECT JOB , SALARY FROM EMP WHERE NAME = 'A' OR NAME = 'B');
```

**总结：使用表子查询主要是多个条件，然后将条件分别分开，然后用一个sql语句的子查询来完成任务。**

## 多表查询练习

```sql
SELECT E.NAME,E.AGE,E.JOB,D.NAME FROM EMP E ,DEPT D WHERE E.DEPT_ID = D.ID
```

