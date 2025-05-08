---
title: Linux编程基础期末复习
categories:
  - 408
  
abbrlink: 2701
date: 2024.11.25
tags: 
   - Linux 
---

# 真题在现

## 概述题

1、简述 GNU、GPL 的含义，及其对 Linux 的影响。

GNU 计划开始于 1984 年 1 月。其创始人是理查德•马修•斯托曼（Richard Matthew 
Stallman）。“GNU”是“GNU's Not UNIX”的递归首字母缩写词。“GNU”的发音为 g'noo。
GNU 计划的目的是开发一个完全类似于 UNIX 的免费操作系统。其内核 Hurd 的开发工作始于
1990 年，但是至今尚未成熟。GNU 计划代表性的产品包括 GCC、Emacs、Bash Shell、GLIBC
等，这些都在 Linux 中被广泛使用。（3 分）
GPL（GNU General Public License，GNU 通用公共许可证）于 1985 年被提出。GPL 试
图保证您共享和修改自由软件的自由。GPL 适用于大多数自由软件基金会的软件。GNU 计划
一共提出了 3 个协议条款：GPL，LGPL（GNU Lesser General Public License，GNU 较宽松
公共许可证），GFDL（GNU Free Documentation License，GNU 自由文档许可证）。（4 分）

**这个题真就是概念的问题，真的很难记**

2.简述进程状态及其转换。

运行（running）态：进程占有处理器正在运行。
就绪（ready）态：进程具备运行条件，等待系统分配处理器以便运行。
等待（wait）态：又称为阻塞（blocked）态或睡眠（sleep）态，指进程不具备运行条件，正
在等待某个事件的完成。（4 分）

还要画个图

**wait状态可以变成ready状态，然后再变成running状态，或者是由running状态转换成wait或者ready状态**

3、Linux 目录结构与 Windows 有何不同？

(1). Linux 中文件名是区分大小写的，所有的 UNIX 系列操作系统都遵循这个规则。（2
分）
(2). Linux 文件通常没有扩展名。给 Linux 文件设置扩展名通常是为了方便用户使用。
Linux 文件的扩展名和它的种类没有任何关系。例如，zp.exe 可以是文本文件，而 zp.txt
也可以是可执行文件。（2 分）
2
(3). Linux 中没有盘符的概念（如 Windows 下的 C 盘）。Linux 的目录结构为树状结构，
顶级的目录为根目录“/”。其他目录通过挂载可以将它们添加到目录树中。例如，对于文件
zp.txt，它在 Linux 中的绝对路径可能是/home/john/zp.txt，而在 Windows 中的绝对路径
可能是 E:\document\zp.txt。（3 分）

4.用户账户的配置文件有哪些？它们各自用途如何？（7 分）
参考答案：
用户账户管理主要涉及 passwd 和 shadow 两个配置文件（3 分）。
passwd 是系统识别用户的一个重要文件，Linux 操作系统中所有的用户都记录在该文件
中（2 分）。
文件 shadow 是文件 passwd 的影子文件。主要保存用户密码配置情况（2 分）。

5.若使用 rmdir 命令来删除某个目录，但无法成功，请说明可能的原因。（7 分）

此目录可能不存在，（2 分）
或者目录不为空，（2 分） 
或者目录中有隐含文件存在，（1 分） 
或者目录被设置了权限，（1 分）
解决方法就是先修改权限，在使用 rm -r 命令删除。（1 分）

**桀桀桀，或者是直接rm -rf直接强制删除，管他啥呢**

## 实操题

1、 完成以下进程管理操作（5 分）
（1）用 vi 编辑 test.txt 文件，然后使用 ctrl+z 将其挂起。
（2）查看当前进程的状态。
（3）将 test.txt 进程（假设进程 pid 为 36357）的优先级设置为 8，再终止该进程。
（4）查看 CPU 和内存的使用情况和进程状态。

```
vi test.txt ctrl+z
ps -l
renice 8 pid
kill -9 pid
top
```

2、当前用户为管理员，且在根目录下，完成以下文件和目录操作（5 分）
（1）在/mnt 目录下创建三个文件夹，dir1、dir2、dir3，并查看是否创建成功；
（2）在 dir1 文件夹内创建 test.sh 文件，并查看是否创建成功；
（3）将/mnt/dir1 下 test.sh 文件复制到 dir2 目录下，复制后的新文件名为 newtest.sh,并查看
是否复制成功；
（4）给文件 newtest.sh 添加可执行权限。
（5）删除文件 newtest.sh。

```
mkdir /mnt/dir{1..3}
ls /mnt
touch /mnt/dir1/test.sh
ls /mnt/dir1
cp /mnt/dir1/test.sh /mnt/dir2/newtest.sh
ls /mnt/dir2
chmod 700 /mnt/dir2/newtest.sh
rm -rf /mnt/dir2/newtest.sh
```

3、请写出以下操作的完整指令（包括命令选项及参数）：假设你是系统管理员，需要增加一
个新的用户账号 Jack，为新用户设置初始密码，锁定用户账号 Jerry，并删除用户账号 Kate。
（5 分）

```
useradd Jack
passwd Jack
passwd -1 Jack
usedel Jack
```

4 、执行命令 ls -1 时，某行显示如下:
-rw-r--r-- 1 teacher teacher 744 3 月 18 11:58 studentdata
(1)用户 teacher 对该文件具有什么权限?
(2)执行命令 useradd student 后，用户 student 对该文件具有什么权限?
(3)如何使任何用户都可以读写执行该文件?
(4)如何把该文件所有者改为用户 root ?
（5 分）

```
读写

只读
chmod 777 stduentdata
chown root studentdata
```

5、叙述 Linux 虚拟机如何增加一块硬盘（设备名为/dev/sdb，文件系统格式 ext4)，如何
实现开机自动挂载硬盘。写出详细的步骤和相关命令。（10 分）
参考答案：

```
先虚拟机新建一块硬盘
分区fdisk /dev/sdb
格式化 mkfs -t ext4 /dev/sdb1
挂载挂载， 先创建一个/home/newdisk,挂载 mount /dev/sdb1 /home/newdisk 
然后设置自动挂载
设置自动挂载，打开/etc/fstab 添加一行 
/dev/sdb1 /home/newdisk ext4 defaults 0 0 
```

## 编程题

1.创建一个 shell 脚本，它从用户那里接收 20 个数，并显示已输入的最小的数。（10 分）

```
#!/bin/bash
min=2147483647
for((count=0;count<20;count=count+1))
do
echo -n "输入数字:"
read value
if [ $min -gt $value ]
then
min=$value
fi
done
```

2. 编写 shell 脚本程序来分析某班某一课程平均成绩，从键盘输入若干个考试分数（分数为
整数），计算并输出所有分数的平均值。要求如下：
（1）使用 while 或 until 循环实现整数输入，如果输入为字符 q，则退出键盘输入；
（2）如果输入分数不在 0—100 范围内，提示用户输入有误；
（3）平均值计算不需要考虑小数部分，只需要得到整数部分即可。
（10 分）

```
#!/bin/zsh
sum=0
count=0
while true;
do
	read -p "输入一个整数" score
	if [[ $score=="q" ]];then
	break
	elif [[ !"$score" =~^[0-9]+$ ]];then
	echo "输入错误"
	elif(( score <0|| score >100 ));then
	echo "输入错误"
	else
	sum=$((sum+score))
	count=$((count+1))
	fi
	done
	if((count == 0));then
	echo "没有输入"
	else
	avg=$((sum/count))
	echo "平均分为"
	fi
```

# 作业再现复习

1.什么是shell? 它有什么作用?
2.简述管道的用途.
3.重定向是什么?有哪些常见类型?
4.获取Linux命令行帮助信息的方法有哪些?
5.简述命令行命令的语法格式。

```
shell是一个用户和操作系统内核之间的一个接口，类似与命令解释器，
输入命令，shell负责解析命令，启动程序和进程等操作
作用：
命令执行
脚本编程
文件操作
进程管理

|管道符号
将多个命令连接起来，前面命令的输出作为下一个命令的输入，用来连接两个或者更多的命令。也可以用来过滤数据

重定向是将命令的输入和输出重定向到文件或者设备中，有>> << > < 2>>
2>这些等

获取帮助
man ls
ls --help
info ls
whatis ls

格式是：
命令 选项 参数
```

1.常见的Linux文件类型有哪些?
2.用户账户的配置文件有哪些?各有什么用途?
3.简述用户账户配置文件的记录格式。
4.组账户的配置文件有哪些?它们各自有什么用途?
5.简述组账户配置文件的记录格式。

```
普通文件，目录，符号连接，字符设备文件，管道文件，块设备文件

/etc/passwd 基本信息
/etc/shadow 密码等
/etc/group 组的信息

/etc/passwd
用户名:密码:UID:GID:描述:家目录:默认Shell
/etc/shadow
用户名:密码:最后修改时间:最小密码年龄:最大密码年龄:警告期:不活动期:过期时间
/etc/group
组名:密码:GID:成员列表

/etc/group：存储组的信息，包括组名、GID 和组成员列表。
/etc/gshadow：存储与组相关的密码信息（通常为空）。

/etc/group 的记录格式为：
组名:密码:GID:成员列表
/etc/gshadow 的记录格式为：
组名:密码:管理员列表:成员列表
```

1.简述Linux软件包管理的发展历史。
2.常见的软件包安装方法有哪些?
3.简述apt命令的基本用法。
4.简述基于源代码的程序安装流程
5.使用apt命令安装apache。
6.使用apt命令安装vim。

```
Linux 软件包管理工具相对简单，用户通常需要手动下载并编译源代码。
二进制包管理：随着 Debian 和 Red Hat 的出现，二进制包管理系统（如 .deb 和 .rpm）得到了广泛采用。
现代化管理：如 apt、yum 和 zypper 等工具使得软件包管理更加简化，支持自动更新和依赖关系管理。

源代码编译
.deb或者是rpm安装
第三方安装工具

apt install 包名
apt update 更新
apt grade 升级
apt remove 包名

下载源代码
解压
cd进入
./configure
make 
sudo make install
如果少包的话，那就进行安装

sudo apt update
sudo apt install apache2
sudo apt update
sudo apt install vim

```

1.Shell的变量类型有哪几种?
2.简述Shel1脚本的常见方式。
3.简述She11分支结构的实现方式。
4.简述She1l循环结构的实现方式。
5.设计一个Shell程序，添加一个新组group1，然后添加个组的50个用户，用户名的形式为stu**，**从01-50。

6.设计一个Shel1程序，该程序能接收用户从键盘输入的数，然后求出其总和、最大值和最小值。

```
位置参数，环境变量，局部变量，特殊变量，只读变量

直接执行，交互执行，脚本中嵌入命令，解释器执行

if-elif-else
case

for,while,until

#!/bin/bash
sudo groupadd group1
for i in $(seq -f "%02g" 1 50)
do 
	sudo useradd stu$i -g group1
	sudo passwd -d stu$i
done

#!/bin/bash
sum=0
max=-9999999
min=9999999

while true; do
    read -p "请输入数字 (或输入'q'退出): " num
    
    if [ "$num" == "q" ]; then
        break
    elif ! [[ "$num" =~ ^[0-9]+$ ]]; then
        echo "无效输入，请输入一个数字！"
        continue
    fi
    
    sum=$((sum + num))
    
    if [ $num -gt $max ]; then
        max=$num
    fi
    
    if [ $num -lt $min ]; then
        min=$num
    fi
done

echo "总和: $sum"
echo "最大值: $max"
echo "最小值: $min"
```

1.使用mkdir创建一个父目录不存在的目录时，需添加什么参数?
2.Linux操作系统中有哪些常见的文件类型?
3.使用什么命令可以删除包括子目录的目录?
4.Linux目录结构和windows目录结构有何不同?
5.简述软链接文件和硬链接文件的区别。

```
mkdir -p /home/memgnankk/aaa

普通文件
块设备文件
目录文件
符号链接
管道文件
字符设备文件
套接字文件

rm -rf /home/mengnankk/temp

Linux的根目录/，所有的文件都从/开始
windows有各个驱动器分区，而Linux只有一个跟目录
Linux分隔符是/
windows是\

软链接是一个指向原文件的链接，删除原文件后失效,ln- s创建
硬链接是指向文件数据块的，相对于另一个名字，删除原文件后不会失效，直到硬数据全部删除
使用ln创建
```

1.简述进程的分类。
2.PID是什么，如何查看进程的pid?
3.如何向进程发送信号?如何结束进程?
4.如何调整进程的优先级?
5.常见的进程启动方式有哪些?
6.如何使用top命令监控进程的运行状态?

```
前台进程，后台进程，守护进程

Pid是操作系统给每个进程分配的唯一进程符号，用ps aus 或者top查看

kill pid 删除进程 kill -9 强制删除 -15优雅的删除

renice -n 10 -p pid直接调整
修改正在进行的程序的优先级
nice -n 10 command 命令的优先级

命令直接启动
命令 &来启动
nohup 命令来启动

top直接查看包括 CPU 使用率、内存占用等信息

```

1.请解释磁盘分区的含义?
2.请解释格式化的含义?
3.新磁盘在可以进行文件存取之前需要经过哪些操作?
4.简述Linux磁盘设备命名方法。
5.简述Linux磁盘分区命令方法。
6.简述MBR和GPT的区别。

```
磁盘分区是指将一个物理硬盘分割成多个逻辑区域，每个区域可以作为一个独立的磁盘来使用。每个分区可以用于存储不同类型的数据（如操作系统、数据文件、备份等），从而提高磁盘的管理效率和数据安全性。分区还可以帮助操作系统更好地管理硬盘资源，如文件系统的创建、文件存取等。

格式化是对磁盘或者分区进行初始化的的过程，在磁盘上创建一个文件系统，然后情空之前的其他数据

先虚拟机新建一块硬盘
分区fdisk /dev/sdb
格式化 mkfs -t ext4 /dev/sdb1
挂载挂载， 先创建一个/home/newdisk,挂载 mount /dev/sdb1 /home/newdisk 
然后设置自动挂载
设置自动挂载，打开/etc/fstab 添加一行 
/dev/sdb1 /home/newdisk ext4 defaults 0 0 

dev/sda、/dev/sdb：表示SATA、SCSI或USB等磁盘设备，a、b、c等字母表示磁盘的顺序。
/dev/sda1、/dev/sda2：表示 /dev/sda 磁盘上的第一个和第二个分区。
/dev/nvme0n1：表示 NVMe 固态硬盘（SSD）设备，n1 是该设备的标识符。
/dev/loop0、/dev/loop1：表示环回设备，通常用于挂载ISO文件等。

fdisk/parted/lsblk

MBR 主要用于旧版操作系统（如Windows 7之前的版本），且支持的磁盘容量有限（最大2TB）。
GPT 是现代磁盘的标准，支持大容量磁盘，并且支持更多的分区，适用于较新版本的操作系统（如Windows 10、Linux等）。
```

# 补充

`lsof`（List Open Files）是一个用于列出进程打开的文件的命令，可以显示哪些文件被哪些进程占用。

```
lsof
lsof /path/to/file  # 查看某个文件被哪个进程打开

```

Linux系统中，用于将标准输出和标准错误重定向到文件的符号是

```
&>
```

Linux系统中，将标准错误重定向到文件里的符号是

```
2>
```

**free命令可以显示系统的内存使用情况**

**df是查看硬盘的使用情况**

`diff` 是用于比较文件内容差异的命令，它会显示两个文件之间的不同之处。常用于源代码管理、配置文件比较等场景。

```
diff file1.txt file2.txt  # 比较 file1.txt 和 file2.txt 文件的差异

```

`ls -i` 命令用于显示文件或目录的 inode 信息。它会在文件名旁边显示该文件的 inode 号

stat令用于显示文件的详细信息，包括文件的大小、权限、时间戳、inode 等，但它并不是专门用于查看 inode 信息的命令。

`lscpu` 是一个用于显示 CPU 架构信息的命令，它提供了有关 CPU 的简明概览，包括架构类型、核心数、线程数、CPU 架构、时钟速度等信息。

## 补充简答

在 Linux 中查看设备信息，可以通过多种命令来查看不同类型的硬件设备信息，如 CPU、内存、硬盘、网络设备等

```
lscup
free -h
lsblk
df -h 
sudo fdisk -l 
ipconfig
ip addr 
lspci -v
lsusb
sudo lshw
uname -a
sudo dimidcode
lspci |grep VGA
ls /dev
sudo parted -l


```

简述 Linux 中文件系统的卸载选项

```
Linux 中，卸载文件系统使用 `umount` 命令
其实就是取消挂载
-l是懒卸载，就是自动卸载
-f是强制卸载
-a 是卸载所有
```

怎样在 Linux 中为内核模块传递参数?

```
在 Linux 中，可以通过 `modprobe` 命令或在 `/etc/modules-load.d/` 配置文件中为内核模块传递参数。
sudo modprobe my_module param=5
options my_module param=5
也可以将模块参数配置在系统文件中，以便在系统启动时自动传递。将配置写入 /etc/modprobe.d/ 目录下的某个文件，如 my_module.conf。
```

