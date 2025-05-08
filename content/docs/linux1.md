---
title: Linux基础1
categories:
  - 408
  
abbrlink: 2701
date: 2024.8.24
tags: 
   - Linux
---

# Linux 基础

## 概述

是一个开源免费的操作系统是一个稳定安全的操作系统

主要的发行版有这些

- Ubuntu
- Cenos
- RedHat
- Debain
- Fedora
- SuSE
- OPenSuSe

linux主要是一个内核，在里面加上东西，就是发行版

**最主要是用在服务器上**，免费稳定高效的特点。

Linux和Unix是有很大关系的

## 网络连接

在一个网段的机器可以相互通信

分为三种方式

- 桥接模式 虚拟系统可以和外部系统相互通讯但是容易造成ip冲突
-  Nat模式 网络地址转换 主机上会生成一个虚拟的网卡，会产生另一个ip。和主机之间会有一个连接，可以上其他的内网公网。不会造成ip冲突。但是其他内网的机器不能访问虚拟机
- 主机模式 就是一个独立的系统，不会和外部进行联系 

## 目录

是采用层级式的树状目录结构

根目录式"/"然后在此目录下再创建其他的目录

在根目录下面式子目录，这些目录是规定好的，一般不能更改

**在linux世界中，一切皆文件**

**linux会把硬件映射成一个文件来管理**

- bin 常用的指令
- etc 配置环境文件
- home 文件桌面等等
- root admin所用的文件
- boot Linux启动的一些核心文件
- dev 设备管理器，一般是放硬件被映射的文件的
- sbin 系统管理员使用的一些命令地方
- lost+found 一般是空的，非法关机里面就会有文件
- lib 基本的动态共享库，类似于windows里面的dll文件的存放处
- user 用户的应用和文件放在这个目录下面
- proc  一个虚拟的目录，系统内存的映射 不能改动
- sys 一个存储内核的 不能改动
- mnt 让用户临时挂在别的文件系统
- media 识别一些设备进行挂载的文件
- tmp 一些临时文件
- opt 主机额外软件安装目录使用的文件夹
- var 存放不断扩充的一些东西 例如日志
- user/local 主机额外安装的目录，一般是通过编译源码的方式安装的程序
- /seliunx [security-enhanced liunx] 一种安全子系统，保证系统安全的

linux的目录系统就和树一样

## 远程登录

正式上线的项目是在公网上的，开发测试可能在内网进行

远程登录使用xshell 文件上传使用xftp

上传一般使用21端口

## vi/vim

vi是一款文本编辑器，linxu自带的

vim是vi一款增强版，会补充一些功能

这两款快捷键差不多

模式切换

- 插入 按i即可进入编辑模式
- 命令行模式 读取等等功能

一般就是进入后按i进入编辑，编辑完之后

按esc 然后输入:wq保存并退出

- :wq 保存并退出
- :q退出
- :q!强制退出，并且不保存

快捷键使用：

- 拷贝当前行 yy 然后p粘贴，几行就几yy

- 删除当前行 dd

- 在文件种查找某个单词 （命令行下输入**/关键词**，这里面是区分大小写的，回车查找，输入N就是查找下一个）

- 取消文件中的行号，设置文件中的行号（:set nu :set nonu)

- 在一般模式下G下面一行，gg回到首行

- 撤销操作 在一般模式下输入u撤销操作

- 快递定位到某一行 在一般模式下，行数+shift+g

  

## 开机重启命令

- shutdown -h now 立刻关机
- shutdown 默认代表 shutdown -h 1 一分钟之后关机
- shutdown -r now 重启
- halt 关机
- reboot 立刻重启
- sync 内存数据同步到磁盘

一般这些关机命令啥的都已经执行了sync了

登录少用root权限

sudo su 可以切换root权限

或者su root

logout 注销用户，在图形界面无效，在运行级别3下才有效

## 用户管理

Linux是一个多用户任务的操作系统

useradd name来添加name用户，每个用户都有一个家目录。

默认是home目录下

或者是useradd -d 指定目录 用户名 ，给新创建的用户指定家目录

修改密码：

passwd name修改用户名的密码，不加name是修改当前用户的密码

删除用户

删除用户但是保留目录： userdel name 

全都删掉：userdel -r name

id name 查询用户

su -name 切换用户

细节：**高到低不需要，低到高需要密码**

whoami查看当前用户，显示的是登录的用户名

### 用户组

没有组的用户自动创建一个自己的组

对权限相同的用户统一管理

groupadd name 	添加组

groupdel name 	删除组

useradd -g 用户组 用户名      添加用户到用户组

usermod -g 用户组 用户名 	移动用户到另一个组中

用户和组相关的文件

- /etc/passwd

用户名：口令：标识：标识组号：注释描述：主目录：登录shell

- /etc/shadow

登录名：加密口令：最后一次修改世界：最小间隔：最大间隔：警告时间：不活动时间“失效时间：标志

- /etc/group

组名：口令：组标识符：组内用户列表（隐藏）

## 文件管理

### 基本命令

ls 列出目录

- -l 显示更加详细
- -a 显示所有隐藏文件夹
- -d 仅列出目录本身

参数可以组合使用

```
ls -al
```

cd 切换目录

- cd ~回到home目录
- cd ..返回上一级目录

pwd 显示当前所在的工作目录

- -p 显示确定的路径

mkdir 创建新的目录

- -r 可以递归创建
- -p 创建多层目录

rm 删除目录

- -r 递归删除
- -rf递归强制删除

cp 复制文件和文件夹

```
cp 源 目的
```

- -r 递归复制

mv 移动

```
mv 源 目的
```

- -f 强制

touch 创建文件 

```
touch mytxt{1..3}
```

可以进行批量创建

### 文件的内容的查看

cat 从第一行开始显示

tac 从最后一行开始显示

more 一页一页的翻动

less 显示内容，一般用于比较大的，但是这个比较舒服

- pageup 和pagedown 用来上下翻动 

- 空格翻动下一行
- /从上搜索 ？从下搜索 
- q 离开

head 和tail一个是前几行一个是后几行

```
[root@www ~]# head -n 20 /etc/man.config

```

加上-n可是显示行号

 **">"是覆盖  ">>"是追加**

ln 软连接，链接到其他文件的路径，类似与一个快捷方式

```
ln -s 源文件 地址/软连接name
```

在使用pwd的时候显示的仍然是 软链接所在的目录

histort 查看历史的命令

- 连续两条一样的只会记录一次
- !5执行曾经执行的5号指令

## 时间

date显示当前日期

date +%Y...其他时间单位相同

```
date +"%Y-%m-%d %H:%M:%S"

```

显示年月日时分秒

## 查找文件

find 命令

有三种查找的方式

- -name  根据名字进行查找

  ```
  find /path/to/directory -name "filename"
  ```

- -size 根据大小进行查找 +表示大于 = 表示等于 -表示小于

```
find /path/to/directory -size +500M
```

- -mtime 根据时间进行查找

```
find /path/to/directory -mtime -7
```

在find命令中可以使用统配符*

例如：

```
find /path/to/directory -name "*.log" -exec rm {} \;

```

是查找.log的文件，然后进行删除。其实就是删除日志

locate命令

这个命令的查找速度很快，但是第一次执行命令前要进行数据库的配置

```
updatedb
```

然后直接搜索，就可以实现快速搜索了

qwq

```
root@Ubuntu:/home/mengnankk# locate hello.txt
/home/mengnankk/hello.txt
```

在进行文件查找的时候可以使用**|grep 进行条件过滤**

- -n 显示行数
- -i 忽略字母的大小写

## 解压和压缩

tar 解压命令

调用 `tar` 命令，处理归档文件。

`-c`：表示压缩文件。

`-x`：表示解压缩文件。

`-z`：表示文件经过 `gzip` 压缩。

`-v`：显示解压缩的过程。

`-f`：指定要操作的文件名。

```
tar -xzvf clashpremium-nightly-linux-amd64.tar.gz
```

gzip gunzip命令，只能用来压缩文件

gzip用来压缩，gunzip用来解压

```
gzip /home/hello.txt
```

解压

```
gunzip /home/hello.txt.gz
```

zip和unzip 命令 能用来压缩目录

用来项目的打包发布很有用

- -r递归压缩，即压缩目录。

```
zip -r myhome.zip /home/
```

将/home/中的所有内容压缩成myhome.zip文件

```
 tar -zxvf /home/myhome.tar.gz -C /opt/tmp2
```

将前面的文件解压到后面去
