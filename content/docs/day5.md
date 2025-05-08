---
title: php命令注入及其实例
categories:
  - web
  
abbrlink: 2701
date: 2023.11.20
tags: 
   - php

---

# php注入概述

rce概念：remote command/code execute，远程命令/代码执行。

php代码执行：在web中，php代码执行是指应用程序过滤不严，用户可通过http请求将代码注入到应用中执行

php代码注入与sql注入比较：

注入的思想类似，均是构造语句绕过服务器的过滤去执行。

区别在于sql注入是将语句注入到数据库中执行，而php代码执行是可以将代码注入到应用中，最终由服务器运行。

php代码注入的条件：

1.程序中含有可以执行php代码的函数或者语言结构

2.传入该函数或者语言结构的参数是可以由客户端控制的（可以直接修改或者造成影响）且应用过滤不严。

危害：

这样的漏洞如果没有特殊的过滤，相当于一个web后门的存在，攻击者可以执行漏洞继承web用户权限，执行任意代码。如果服务器没有正确配置或者web用户权限比较高的话，还可以读写靶机服务器任意文件的内容，甚至控制整个网站或者服务器。

# 相关的函数或语言结构

## 1.eval()函数

作用：该函数的作用是将字符串作为PHP代码执行。

例：

```
<?php
if (isset($_GET['code'])){
	$code=$_GET['code'];
	eval($code);
}else{
	echo "Please submit code!<br >code=phpinfo();";
}

```

通过浏览器访问上述函数所在网页时，可以通过传递参数该code来执行PHP探针。主要方式有以下几种：
①普通方式提交变量：`?code=phpinfo();`
②以语句块方式提交变量：`?code={phpinfo();}`
③以多语句方式提交参数：`?code=1;phpinfo();`

payload:?code=

## 2.assert()函数

作用：该函数的作用是将字符串作为PHP代码执行。如果它的条件返回错误，则终止程序执行。

例：

```
<?php
if (isset($_GET['code'])){
	$code=$_GET['code'];
	assert($code);
}else{
	echo "Please submit code!<br >?code=phpinfo();";
}

```

普通方式提交变量：`?code=phpinfo()`或者`?code=phpinfo();`
与eval()函数有别的是，该函数不能执行传入语句块或多语句作为参数。

## 3.preg_replace()函数

作用：该函数用于对字符串进行正则处理。

解析：preg_replace 函数一个参数是一个正则表达式，按照 php的格式，表达式在两个/之间，如果在表达式末尾加上一个 e，则第二个参数就会被当做 php代码执行

```
<?php @preg_replace("/abcd/e",$_POST['hacker'],"abcdefg"); ?>
```

通过浏览器访问上述函数所在网页时，可以通过传递参数该code来执行PHP探针。主要方式有以下几种：
①普通方式提交变量：?code=[phpinfo();]，[]是由于preg_replace的第一个参数有分号。
②以语句块方式提交变量：?code={[phpinfo();]}
③以多语句方式提交参数：?code=1;[phpinfo();]
