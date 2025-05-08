---
title: Mysql DCL
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.9
tags: 
   - mysql 

---

# DCL(数据库管理)

介绍：数据控制语言，控制数据库的用户，控制数据库的访问

## 用户管理

1.查询用户

```
USE MYSQL;
SELECT * FORM USER;
```

用用户名和主机地址确定一个用户

```
CREATE USER 'USERNAME'@'LOCALHOST' IDENTIFLED BY 'PASSWPRD';
```

任意主机都可访问：

```
CREATE USER 'USERNAME'@'%' IDENTIFLED BY 'PASSWPRD';
```

2.修改用户密码：

```
ALTER USER 'USERNAME'@'主机名' IDENTIFIED WITH MYSQL_NATIVE_PASSWORD BY 'PASSWORD';
```

修改mysql本地连接的密码

3.删除用户

```
DROP USER 'USERNAME'@'主机名';
```

主机可以使用通配符%

## 权限控制

权限：

ALL

SELECT 

INSERT

UPDATE

DELETE

ALTER

DROP

CREATE

1.查询权限

```
SHOW GRANTS FOR 'USERNAME'@'LOCALHOST';
```

2.授予权限

```
GRANT 权限列表 ON DATEBASE_NAME.TABLE_NAME TO 'USERNAME'@'LOCALHOST';
```

3.撤销权限

```
REVOKE 权限列表 ON DATEBASE_NAME.TABLE_NAME FROM 'USERNAME'@'LOCALHOST';
```

4.

```
GRANT ALL PRIVILEGES ON *.* TO 'mengnankk'@'localhost' WITH GRANT OPTION;
```

![](https://imgbed.mengnankk.asia/202407081551504.jpg)