---
title: Linux shell编程基础语法
categories:
  - 408
  
abbrlink: 2701
date: 2024.9.26
tags: 
   - Linux
   - shell
---

# shell简介

shell是一个命令行解释器，就是给Linux内核发送一个请求，然后返回某些东西。可以用来编写命令脚本等

## shell脚本的格式要求

bash脚本开头要以#!/bin/bash开头

脚本要有可执行的权限

而zsh就要以zsh开头了

脚本启动可以./test.sh启动，或者是

```
sh test.sh
```

## shell变量

shell变量分为系统变量和用户变量

系统变量是$HOME/$PWD等等

```
set
```

显示当前shell的所有变量

```
status=0
sysparams
termcap
terminfo
userdirs
usergroups
watch=(  )
widgets
zle_bracketed_paste=( $'\C-[[?2004h' $'\C-[[?2004l' )
zsh_eval_context=( toplevel )
zsh_scheduled_events
```

我的zsh Shell中一部分变量

```
☁  ~  echo $HOME
/home/mengnankk
☁  ~  echo $PWD 
/home/mengnankk
☁  ~  echo $JAVA_HOME
/usr/lib/jvm/java-11-openjdk-amd64

```

打印部分变量

定义变量

```
变量=value
```

撤销变量

```
unset 变量
```

声明静态变量，静态变量是不能unset的

```
readonly 变量=value
```

### 语法注意

- 变量名称可以由字母数字下划线组成，开头不能是数字

- 等号的两边不能有空格

- 变量名称一般习惯大写
- 命令引用可以是用``反引号，或者使用$(date)

### 环境变量的设置

```
export 变量名=value
```

用来设置环境变量

```
sudo vim ~/.bashrc
```

在这里面编辑

保存之后

```
source ~/.bashrc
```

```
:<<! 内容!
```

这是shell的标准注释

### 参数变量

就相当于**传参**

```
#!/bin/zsh
echo "0=$0 1=$1 2=$2"
echo "所有的参数=$*"
echo "$@"
echo "参数的数量是$#"

```

$*和$@都是代表全部的参数

$#代表的是参数的熟练

$数字，代表是各个参数

**记得给sh一个运行的权限**

```
☁  shellcode  ./shell1.sh 100 800      
0=./shell1.sh 1=100 2=800
所有的参数=100 800
100 800
参数的数量是2
```

### 预定义变量

就是shell设计者事先已经定义好的变量

- $$当前的进程的PID
- $!后台运行的最后一个PID
- $?最后一次执行命令的返回值，0代表执行成功，其他代表不能成功

其实就相当于一个**内部变量**

```
☁  shellcode  ./shell1.sh 100 800      
0=./shell1.sh 1=100 2=800
所有的参数=100 800
100 800
参数的数量是2
25382 0 0
```

25382是当前进程的PId

```
#!/bin/zsh
echo "0=$0 1=$1 2=$2"
echo "所有的参数=$*"
echo "$@"
echo "参数的数量是$#"
echo $$ $! $?
/home/mengnankk/shellcode/shell1.sh &
echo $!
```

/home/mengnankk/shellcode/shell1.sh &在后台运行这个程序

## 运算符

$((运算式))或者是$[] 

再或者是expr m + n 这个是expression表达式，m和n之间都要有空格的

而且在expr中  

```
\* / \%
```

分别是乘 除 取余

实例：

```
#!/bin/zsh
r=$[2+3]
temp=`expr 2 + 4`
res=`expr $temp \* 4`
echo "ress=$res"
echo "res=$r,s=$temp"

```

return

```
☁  shellcode  ./shell2.sh  
ress=24
res=5,s=6
```

得到结果

注意几个问题：

- expr反引号前后都不要有空格，但是运算符和数字之间要有空格
- 引用变量的时候必须用$

## 运算结构

### if

[ condition ] **条件内要和中括号之间要有空格**

非空就会返回true，空则返回0

可以使用**$?**进行验证，看是否成功

判断语句

- 字符串之间比较用=
- 两个整数进行比较
  - -lt小于
  - -le小于等于
  - -eq等于
  - -gt大于
  - -ge大于等于
  - -ne不等于
- 按照文件的权限进行判断
  - -r
  - -w
  - -x
- 文件的类型
  - -f文件存在并是一个常规的文件
  - -e文件是否存在
  - -d文件是否存在并且是一个目录

实例：

```
#!/bin/zsh
if [ "ok" = "ok" ]
then
        echo "eq"
fi
if [ 23 -ge 8 ]
then
        echo "da yu"
fi
if [ -f /home/mengnankk/shellcode/aaa.txt ]
then
        echo "cun zai"
fi

```

return

```
☁  shellcode  ./if.sh
eq
da yu
```

**在shell中即使条件为空，【】两边也要空格才行**

### elif

**多分支就是if+elif就行了**

里面的条件和前面的if一样，【】里面都需要前后两个空格

### case

```
case $变量名 in "value1" )

echo " "

;;
```

```
#!/bin/zsh
case $1 in
"1")
        echo "a"
        ;;
"2")
        echo "b"
        ;;
*)
        echo "other...."
        ;;
esac

```

return

```
☁  shellcode  ./case.sh
other....
☁  shellcode  ./case.sh 2
b
☁  shellcode  ./case.sh 1
a
```

### for循环

```
for 变量 in value1 value2...
do
shell语句
done
```

或者是

```
for((初始值；循环控制条件；变量变化))
do
shell语句
done
```

```
#!/bin/zsh
for i in "$*"
do
        echo "num is $i"
done

```

在vim里编辑这个代码

```
❯ sh for.sh 12 11 55
num is 12 11 55
```

输出

**注意：这里就能看出$*和$@的区别了**

$*是把所有的变量当作一个整体，所以只会输出一次

$@不是把所有变量当作一个整体，所以就会分开输出

```
❯ sh for.sh 12 11 55
num is 12
num is 11
num is 55
```

为$@的输出结果

```
#!/bin/zsh

# 输出传入的参数
for i in "$@"; do
    echo "num is $i"
done

# 计算 0 到 100 的总和
sum=0
for ((i=0; i<=100; i++)); do
    sum=$((sum + i))  # 计算总和
done

echo "$sum"

```

另一种

```
❯ zsh for.sh
5050
```

完成！

### whilie

```
while [[ condition ]]
do
shell语句
done
```

这是有条件的while循环语句

无限循环while

```
while :
do
shell语句
done
```

与之相对的还有until命令

```
until [[ condition ]]
do
shell语句
done
```

实例

```
#!/bin/zsh
a=50
while [[ $a -gt 49 ]]
do
    echo "da yu 49"
    break  # 添加 break 防止死循环
done

until [[ $a -eq 50 ]]
do
    echo "aaa"
    break  # 添加 break 防止死循环
done

```

return 

```
❯ ./while.sh
da yu 49
```

### read

- -p 指定读取值的提示符
- -t 指定读取值的变量值

read 选项 参数

```
read -p "请输入一个数num1=" num1
```

可以输入多个变量，中间空格间断

```
read -t "请输入一个数num1=" num1
```



## 字符串

字符串是shell编程中最常用最有用的数据类型

可以用单引号双引号

单引号：

```
str='this is a string'
```

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的
- 单引号字串中不能出现单引号（对单引号使用转义符后也不行）

双引号：

```
your_name='qinjx'
str="Hello, I know your are \"$your_name\"! \n"
```

- 双引号里可以有变量
- 双引号里可以出现转义字符

### 对于字符串的操作

拼接字符串：

```
your_name="qinjx"
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"

echo $greeting $greeting_1
```

获取字符串的长度：

```
string="abcd"
echo ${#string} #输出：4
```

提取子字符串：

```
string="alibaba is a great company"
echo ${string:1:4} #输出：liba
```

查找子字符串：

```
string="alibaba is a great company"
echo `expr index "$string" is`#输出：3，这个语句的意思是：找出字母i在这名话中的位置
```

```
#!/bin/zsh
a="aaa54545 948484 dsfdiufhdf"
echo `expr index "$a" is`
```

return 

```
❯ ./find.sh
18
```

查找其他的字符的例子

```
#!/bin/zsh
a="aaa54545 948484 dsfdiufhdf"

# 查找字母 'a'
pos_a=$(expr index "$a" a)
echo "位置 of 'a': $pos_a"

# 查找字母 'd'
pos_d=$(expr index "$a" d)
echo "位置 of 'd': $pos_d"

# 查找字母 'i'
pos_i=$(expr index "$a" i)
echo "位置 of 'i': $pos_i"

```

## 函数

### 系统函数

basename获取最后/后的部分，常用于获取文件名

```
basename /home/mengnankk/aaaa/test.txt
```

```
❯ basename /home/mengnankk/aaaa/test.txt
test.txt
```

```
❯ basename /home/mengnankk/aaaa/test.txt .txt
test
```

dirname返回最后/路径前的部分，获取路径

```
❯ dirname  /home/mengnankk/aaaa/test.txt
/home/mengnankk/aaaa
```

### 自定义函数

```
[ fuction ] funname[()]
{
action;
[return]
}
```

```
#!/bin/zsh

function getName() {
    local sum=$((n1 + n2))
    echo "$sum"
}

echo -n "n1="
read n1
echo -n "n2="
read n2
getName

~  
```

非交互模式下不能使用read -p选项

```
❯ ./getName.sh
n1=1
n2=1
2

```

# shell脚本的综合使用

结合grep awk 和sed的例子

一个名为 `data.txt`的文件的例子

```
Alice,25,Engineer
Bob,30,Designer
Charlie,22,Intern
Diana,35,Manager
```

任务：从这个文件中找出年龄大于 25 岁的人，并将他们的姓名和职业提取出来。

```
grep -E '^[^,]+,[2-9][6-9]|[3-9][0-9]' data.txt
```

找到年龄大于25

```
grep -E '^[^,]+,[2-9][6-9]|[3-9][0-9]' data.txt | awk -F, '{print $1 ", " $3}'
```

提取姓名和职业

```
grep -E '^[^,]+,[2-9][6-9]|[3-9][0-9]' data.txt | awk -F, '{print $1 ", " $3}' | sed 's/^/"/; s/$/"/'
```

格式化输出
