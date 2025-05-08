---
title: Linux shell编程进阶
categories:
  - 408
  
abbrlink: 2701
date: 2024.10.18
tags: 
   - Linux
   - shell
---

# 基本脚本

### echo 使用

```bash
# 输出普通字符串
echo "hello, world"
# Output: hello, world

# 输出含变量的字符串
name=zp
echo "hello, \"${name}\""
# Output: hello, "zp"

# 输出含换行符的字符串
echo -e "YES\nNO" 
# Output:
# YES
# NO

# 输出内容定向至文件
echo "test" > test.txt

# 输出执行结果
echo `pwd`
# Output: (当前目录路径)
```

示例脚本

```bash
#!/bin/zsh
echo "aaa,mmmmmmmm"
echo -e "aaaa\naaaa"
name=woshini
echo "name is ${name}"
echo "$(pwd)"
```

**输出:**
```
aaa,mmmmmmmm
aaaa
name is woshini
/home/mengnankk/shellcode
```

### exit

```bash
#!/bin/zsh
var1=4
var2=6
var3=$(($var1 + $var2))
echo "$var3"
exit 5
```

**输出:**
```
10
```

### printf

```bash
printf '%d %s\n' 1 "abc"
# Output: 1 abc

# 格式化输出
printf "%-10s %-8s %-4.2f\n" 姓名 性别 体重kg
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
```

**输出:**
```
姓名     性别   体重kg
郭靖     男      66.12
```

### expr

```bash
#!/bin/zsh
r=$[2+3]
temp=`expr 2 + 4`
res=`expr $temp \* 4`
echo "ress=$res"
```

**输出:**
```
ress=24
```

### bc

```bash
var1=100
var2=45
var3=`echo "scale=4; $var1 / $var2" | bc`
echo The answer for this is $var3
```

### 字符串操作

```bash
your_name="qinjx"
greeting="hello, ${your_name} !"
echo $greeting
# 获取字符串长度
string="abcd"
echo ${#string} # 输出：4
# 提取子字符串
echo ${string:1:4} # 输出：liba
```

### 数组

```bash
nums=( [2]=2 [0]=0 [1]=1 )
colors=( red yellow "dark blue" )

# 访问数组的单个元素
echo ${nums[1]} # Output: 1

# 获取数组长度
echo ${#nums[*]} # Output: 3

# 向数组中添加元素
colors=( white "${colors[@]}" green black )
echo ${colors[@]} # Output: white red yellow dark blue green black
```

# 信号

## 捕捉信号

```bash
#!/bin/zsh
trap "echo 'sorry'" SIGINT SIGTERM
count=1
while [ $count -le 10 ]; do
    echo "loop #$count"
    sleep 5
    count=$((count + 1))
done
```

## 捕捉脚本的退出

```bash
#!/bin/zsh
trap "echo bybyb" EXIT
count=1
while [ $count -le 5 ]; do
    echo "Loop #$count"
    sleep 3
    count=$((count + 1))
done
```

# 操作符

## 关系运算符

```bash
# 10 -eq 20: x 不等于 y
# 10 -ne 20: x 不等于 y
# 10 -gt 20: x 大于 y
# 10 -lt 20: x 小于 y
# 10 -ge 20: x 大于或等于 y
# 10 -le 20: x 小于或等于 y
```

## 字符串运算符

```bash
[[ -n string ]] # 判断字符串非空
[[ -z string ]] # 判断字符串为空
[[ string1 = string2 ]] # 判断字符串相等
[[ string1 != string2 ]] # 判断字符串不相等
```

## 文件测试运算符

```bash
-r # 检测文件是否可读
-w # 检测文件是否可写
-x # 检测文件是否可执行
-f # 检测是否为普通文件
-d # 检测是否为目录
-s # 检测是否非空
-e # 检测文件是否存在
```

这份内容结构更加清晰，方便阅读和理解！如果需要进一步修改或添加，请告诉我。

## 逻辑运算符

主要还是&&和||

除了这些还有

**!表示非**

**`-o` 和 `-a`**：在 `[ ... ]` 或 `test` 命令中使用，分别表示逻辑或和逻辑与（不推荐使用，建议使用 `||` 和 `&&`）。

```
bash复制代码[ condition1 -o condition2 ]  # condition1 或 condition2
[ condition1 -a condition2 ]  # condition1 和 condition2
```

# 文件操作

## 创建临时文件

```
#!/bin/zsh
tempFile=`mktemp test.XXXXXX`
exec 3>${tempFile}
echo "This is the first line" >&3
echo "This is the second line" >&3
echo "This is the last line" >&3
exec 3>&-
cat ${tempFile}
rm -f ${tempFile} 2> /dev/null

```

return

```
❯ ./file.sh
This is the first line
This is the second line
This is the last line
```

注意mktemp 命令创建临时文件的时候

**mktemp` 命令要求模板中包含至少 6 个 `X**

## 创建临时目录

```
#!/bin/zsh
tempFile=mktemp -d dir.XXXXXX
cd ${tempFile}|exit 1
tempFile1=mktemp temp.XXXXXX
tempFile2=mktemp temp2.XXXXXX
exec 7> ${tempFile1}
exec 8> ${tempFile2}
echo "This is a test line of data for $tempFile1" >&7
echo "This is a test line of data for $tempFile2" >&8
```

return

创建成功

还有一个改进版

```
#!/bin/zsh

# 创建一个临时目录
tempFile=$(mktemp -d dir.XXXXXX)

# 切换到临时目录
cd "${tempFile}" || exit 1

# 在临时目录中创建临时文件
tempFile1=$(mktemp temp.XXXXXX)
tempFile2=$(mktemp temp2.XXXXXX)

# 打开文件描述符并向临时文件写入数据
exec 7> "${tempFile1}"
exec 8> "${tempFile2}"
echo "This is a test line of data for $tempFile1" >&7
echo "This is a test line of data for $tempFile2" >&8

# 关闭文件描述符
exec 7>&-
exec 8>&-

# 显示临时文件内容
echo "Contents of $tempFile1:"
cat "${tempFile1}"

echo "Contents of $tempFile2:"
cat "${tempFile2}"

# 删除临时文件
rm -f "${tempFile1}" "${tempFile2}"

# 删除临时目录
rmdir "${tempFile}" || echo "Failed to remove directory ${tempFile}: It may not be empty."

```

