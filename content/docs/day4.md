---
title: php学习（2）
categories:
  - web
  
abbrlink: 2701
date: 2023.11.20
tags: 
   - php

---

# web基础知识

## php文件有以下几种备份的格式:

```
.git .svn .swp .bak .bash-history
```

类如

```
index.php.git
```

## robots协议

robots协议规定了哪些东西可以抓取，哪些东西不能够抓取。robots.txt是一个文本文件也是一个协议。其可以在搜索引擎中访问网站中要查看的第一个文件。

## cookie

cookie是主机访问web服务器由服务器创建的，将信息储存在用户计算机上的文件，一般用户习惯使用cookies

cookie的使用：

1..判断用户是否已经登录网站

2.购物车网站用户跳转保存在其中的数据

3.等等等

## php对于文件的读取

```
php://filter/read=convert.base64-encode/resource=index.php
```

### 组成解析

1.php://filter/可以作为一个中间流来处理其他流，可以进行任意文件的读取。

2.read=convert.base64-encode/resource将数据转换为base64编码来读取数据

3.=index.php读取的文件

### 添加：

读取上级目录文件

```
php://filter/read=convert.base64-encode/resource=.../.../a.txt
```

读取其他的目录文件

```
php://filter/read=convert.base64-encode/resource=F：\b.txt
```

