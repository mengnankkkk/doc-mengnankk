---
title: Linux基础2
categories:
  - 408
  
abbrlink: 2701
date: 2024.9.3
tags: 
   - Linux
---

# 组

每一个用户必须属于一个组，不能独立与组外。每个文件有所有者，所在组，其他组的概念

修改文件的所有者

```
chown 用户名 文件名
```

创建组,加入用户

```
groupadd group
useradd -g group username
```

谁创建了文件，文件就属于这个用户所在的组

修改用户的Uid

```
uermod -u 1500
```

一般1000以内是系统用户

用户信息

```
id name
```

改变文件的用户不会改变文件所在的组

改变文件所在的组

```
chgrp 组名 文件名
```

改变用户的组

```
usermod -g 组名 用户名
```

```
usermod -d 目录名 用户名 
```

修改组的UId

```
groupmod -g 2000 name
```

查看在哪个组

```
groups name
```



改变用户初始登录的目录，用户需要有进入该目录的权限才行

# 权限

文件操作组和其他组分别对文件有不同的操作权限

```
drwxrwxrwx  2 mengnankk mengnankk  4096  9月  4 17:04 smbshare/
```

- 第0位代表文件类型

  - l表示链接，相当于快捷方式
  - d表示目录
  - c表示字符设备，相当于文件鼠标键盘
  - b表示这是块设备，比如硬盘
  - -表示隐藏

- 第1-3位确定所有者（文件的所有者）拥有该文件的权限

- 第4-6位确定所属组（同用户组的）拥有该文件的权限

- 第7-9位确定其他用户拥有该文件的权限

  

  **rwx分别代表读写执行**

rwx作用在文件和目录是不同的

在文件中：

- r代表可以读取
- w代表可以修改，但是不一定能删除文件，必须对文件所在的目录有w的权限
- x要是可执行的文件

在目录中

- r代表可读,ls查看目录内容
- w代表可以修改，可以创建删除重命名
- x代表可以进入该目录

可用数字表示为

r=4 w=2 x=1 所以rwx=7

修改权限s

使用chmod修改权限

u=rwx g=rx o=x

```
chmod o+w 文件/目录名
```

```
chmod u-w 文件/目录名
```

直接给全部的权限

```
chmod 777 文件/目录名
```

这样的话每个人都可以用了

修改文件的目录组的

```
chgrp group 文件/目录 
```

修改权限的时候使用**数字**更加方便

# crond任务调度

让系统在某个时间执行响应的命令

crontab命令

- -e  编辑定时任务
- -l 查询crontab任务
- -r 删除任务

```
*/1 * * * * ls -l /etc/ > /tmp/to.txt
```

*的具体含义

- 第一个代表一小时中的第几分钟 0-59
- 第二个代表一天中的第几个小时 0-23
- 第三个代表一个月当中的第几天 1-31
- 第四个代表一年中的第几个月 1-12
- 第五个代表一周中的星期几 0-7 （0和7都代表周天）

*代表任何时间

，代表不连续时间

-代表持续的时间范围

*/n代表隔多久执行一次

**星期几和号一般不要同时出现**

现在有一个脚本my.sh

```
*/1 * * * * /home/my.sh
```

运行my.sh这个脚本

运行实例备份数据库

```
crontab -e
0 2 * * * mysqldump -u root -p mydb > /home/mengnankk/mydb.bak
```

# at定时任务

是**一次性**的定时任务计划

atd守护进制每一分钟检查任务队列

一定要保证atd进程的启动才能让任务进行

```
mengnankk@Ubuntu:~$ ps -ef | grep atd
daemon      4628       1  0 12:36 ?        00:00:00 /usr/sbin/atd -f
mengnan+    4732    4253  0 12:37 pts/0    00:00:00 grep --color=auto atd

```

发现这个进程已经启动

```
at 选项 时间
```

ctrl+d结束命令的输入

1. **-f 文件**
   从指定的文件中读取命令而不是标准输入。例如：

   ```
   bash
   
   
   复制代码
   at -f script.sh 12:00
   ```

   这将在12:00执行 `script.sh` 文件中的命令。

2. **-m**
   任务完成后发送邮件给用户（即使没有输出）。

   ```
   bash
   
   
   复制代码
   at -m 12:00
   ```

1. **-l**
   列出当前用户的待处理任务。这个选项与 `atq` 命令相同，作用是显示已经排队但尚未执行的任务。
   示例：

   ```
   bash
   
   
   复制代码
   at -l
   ```

2. **-d**
   删除指定的任务。这个选项与 `atrm` 命令相同，作用是删除已排队的任务。
   示例：

   ```
   bash
   
   
   复制代码
   at -d 2
   ```

   这里的 `2` 是任务的编号（可以通过 `at -l` 查看任务编号）。

3. **-c**
   显示指定任务的内容。这个选项用来查看某个任务具体会执行什么命令。
   示例：

   ```
   bash
   
   
   复制代码
   at -c 2
   ```

   这里的 `2` 是任务的编号。

4. **-q 队列**
   指定任务所在的队列。默认队列为 `a`，可以通过该选项将任务放入其他队列。
   示例：

   ```
   bash
   
   
   复制代码
   at -q b 12:00
   ```

5. **-v**
   显示任务的执行时间。这将使 `at` 命令输出任务将执行的确切时间。
   示例：

   ```
   bash
   
   
   复制代码
   at -v 12:00
   ```

应用案例

```
mengnankk@Ubuntu:~$ at 5pm + 2 days
warning: commands will be executed using /bin/sh
at Tue Sep 10 17:00:00 2024
at> /bin/ls /home/mengnankk
at> <EOT>
job 1 at Tue Sep 10 17:00:00 2024
```

输入输错了就按ctrl+backspace删除，写完ctrl+d结束

# 磁盘

无论有多少分区，一共就一个根目录

使用了**载入**的处理方法

swap是一个交换区

挂载 mount

硬盘中的一个具体的分区和根目录的一个目录关联起来

```
lsblk
```

查看分区的挂载

```
☁  ~  lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0     4K  1 loop /snap/bare/5
loop1    7:1    0  55.7M  1 loop /snap/core18/2829
loop2    7:2    0  74.3M  1 loop /snap/core22/1586
loop3    7:3    0  74.3M  1 loop /snap/core22/1612
loop4    7:4    0 269.8M  1 loop /snap/firefox/4793
loop5    7:5    0 271.2M  1 loop /snap/firefox/4848
loop6    7:6    0   497M  1 loop /snap/gnome-42-2204/141
loop7    7:7    0  12.9M  1 loop /snap/snap-store/1113
loop8    7:8    0  12.3M  1 loop /snap/snap-store/959
loop9    7:9    0  91.7M  1 loop /snap/gtk-common-themes/1535
loop10   7:10   0  40.4M  1 loop /snap/snapd/20671
loop11   7:11   0  38.8M  1 loop /snap/snapd/21759
loop12   7:12   0   500K  1 loop /snap/snapd-desktop-integration/178
loop13   7:13   0   452K  1 loop /snap/snapd-desktop-integration/83
loop14   7:14   0 321.1M  1 loop /snap/vlc/3777
sda      8:0    0    40G  0 disk 
├─sda1   8:1    0     1M  0 part 
├─sda2   8:2    0   513M  0 part /boot/efi
└─sda3   8:3    0  19.5G  0 part /var/snap/firefox/common/host-hunspell
                                 /
sr0     11:0    1   4.7G  0 rom  /media/mengnankk/Ubuntu 22.04.4 LTS amd64

```

对与ide硬盘来说 hdx~分别的分区

对与scsi硬盘来说是sdx~分别分区

我自己的电脑就是sdx

## 将未分配的空间分配到某个空间的方法

```
sudo fdisk -l
```

查询未分配的空间

使用

```
sudo parted /dev/sda
```

进入parted交互模式

```
resizepart 3 100%
```

全部分配到第三个分区（根目录）

按q退出

然后

```
sudo resize2fs /dev/sda3
```

执行命令确认分配

```
df -h
```

确认已经分配

```

☁  ~  duf
+------------------------------------------------------------------------------------------------------------------+
| 3 local devices                                                                                                  |
+------------------------------------+--------+-------+--------+-------------------------------+------+------------+
| MOUNTED ON                         |   SIZE |  USED |  AVAIL |              USE%             | TYPE | FILESYSTEM |
+------------------------------------+--------+-------+--------+-------------------------------+------+------------+
| /                                  | 147.0G | 21.8G | 119.0G | [##..................]  14.8% | ext4 | /dev/sda3  |
| /boot/efi                          | 512.0M |  6.1M | 505.9M | [....................]   1.2% | vfat | /dev/sda2  |
| /var/snap/firefox/common/host-huns | 147.0G | 21.8G | 119.0G | [##..................]  14.8% | ext4 | /dev/sda3  |
| pell                               |        |       |        |                               |      |            |
+------------------------------------+--------+-------+--------+-------------------------------+------+------------+
+---------------------------------------------------------------------------------------------------+
| 6 special devices                                                                                 |
+----------------+--------+--------+--------+-------------------------------+----------+------------+
| MOUNTED ON     |   SIZE |   USED |  AVAIL |              USE%             | TYPE     | FILESYSTEM |
+----------------+--------+--------+--------+-------------------------------+----------+------------+
| /dev           |   1.9G |     0B |   1.9G |                               | devtmpfs | udev       |
| /dev/shm       |   1.9G |     0B |   1.9G |                               | tmpfs    | tmpfs      |
| /run           | 386.9M |   3.5M | 383.4M | [....................]   0.9% | tmpfs    | tmpfs      |
| /run/lock      |   5.0M |   4.0K |   5.0M | [....................]   0.1% | tmpfs    | tmpfs      |
| /run/snapd/ns  | 386.9M |   3.5M | 383.4M | [....................]   0.9% | tmpfs    | tmpfs      |
| /run/user/1000 | 386.9M | 128.0K | 386.7M | [....................]   0.0% | tmpfs    | tmpfs      |
+----------------+--------+--------+--------+-------------------------------+----------+------------+

```

 

## 虚拟机增加硬盘步骤

1.vm安装硬盘

2.

```
fdisk /dev/sdb
```

更改sdb硬盘

3.

- 输入n新建一个

- 输入p默认新建区
- 按照默认填写
- 然后w写入操作，并退出。不保存按q

4.格式化分区

```
mkfs -t ext4 /dev/sdb1
```

格式化sdb1

ext4为文件类型

5.

```
mount /dev/sdb1 /home/mengnankk/newdir
```

这是进行挂载

不一定非得挂根目录

6.卸载

```
unmount /dev/sdb1
```

或者直接写目录

**用命令行挂载重启会失效**

也就是说仅当前有效

7.永久挂载要修改/etc/fstab实现挂载

执行mount -a 即刻生效

在这个文件中，第一个0表示不做dump备份，第二个0不做检查硬盘扇区

## 磁盘管理

```
df -h 
```

磁盘使用率到80就要清理了

```
du -h /
```

查询目录的使用情况

- -s 汇总
- -h 大小
- -a 含文件
- -c 列出汇总值
- --max-depth=1查询子目录的深度，这是就查询第一层的子目录

```
☁  ~  du -shc /home/mengnankk
1.5G	/home/mengnankk
1.5G	总计
```

使用案例

统计~下文件的个数

```
☁  ~  llh /home/mengnankk |grep "^-"|wc -l
22
```

统计~下文件的个数，包括子目录

```
☁  ~  llh -R |grep "^-"|wc -l
15954
```

默认就是~目录下面的了

**"^d"代表以d开头的，就是代表目录，这是正则表达式的写法**

以树状展示目录名

```
☁  ~  tree /home/mengnankk 
/home/mengnankk
├── clash
│   ├── cache.db
│   ├── clashpremium-nightly-linux-amd64.tar.gz
│   ├── config.yaml
│   ├── Country.mmdb
│   └── CrashCore
├── Desktop
├── Documents
├── Downloads
│   └── 37aa5dc36d88444786b6b543845963ba.jpg
├── Music
├── Pictures
│   ├── Screenshot.png
│   └── uToolsWallpapers
├── Public
├── smbshare
├── snap
│   ├── firefox
│   │   ├── 4793
│   │   ├── 4848
│   │   ├── common
│   │   └── current -> 4848
│   ├── snapd-desktop-integration
│   │   ├── 178
│   │   ├── 83
│   │   ├── common
│   │   └── current -> 178
│   └── snap-store
│       ├── 1113
│       ├── common
│       └── current -> 1113
├── Templates
├── txtdir
│   └── test.txt
└── Videos

```

tree命令需要自己去安装哦

# 网络

 linux中查看自己ip的指令

```
ip addr show
```

在NAT模式中，虚拟机是有一个新的ip的，虚拟机和本机之间考vmnet8进行通讯

vmnet8是NAT模式下对应的一个虚拟网卡

vmnet8只是用来通讯使用的，断开之后是不影响虚拟机上外网的

在其中的无线网卡通过一个网关，然后再去连接到外网，进行各种数据的交换

```
C:\Users\ikeife>ipconfig

Windows IP 配置


以太网适配器 以太网:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

未知适配器 本地连接:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

无线局域网适配器 本地连接* 1:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

无线局域网适配器 本地连接* 2:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

以太网适配器 VMware Network Adapter VMnet1:

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::be75:390:1eb0:4eb6%17
   IPv4 地址 . . . . . . . . . . . . : 192.168.117.1
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . :

以太网适配器 VMware Network Adapter VMnet8:

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::80f0:ad38:b00d:247a%5
   IPv4 地址 . . . . . . . . . . . . : 192.168.98.1
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . :

无线局域网适配器 WLAN:

   连接特定的 DNS 后缀 . . . . . . . :
   IPv4 地址 . . . . . . . . . . . . : 192.168.15.83
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . : 192.168.15.119

以太网适配器 蓝牙网络连接:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

以太网适配器 vEthernet (WSL (Hyper-V firewall)):

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::ca2d:fa0b:d2b3:c4c4%61
   IPv4 地址 . . . . . . . . . . . . : 172.24.64.1
   子网掩码  . . . . . . . . . . . . : 255.255.240.0
   默认网关. . . . . . . . . . . . . :
```

有一个内网ip和一个外网ip，内网ip是这个机器所在局域网的ip

在同一个网关内可以相互通信，即可以**ping**成功，ping就是测试网络是否联通这个主机

或者是直接简写

```
ip a
```

## linux获取ip的方式

1.自动获取

界面设置，不容易重复

2.指定Ip

直接修改配置文件指定ip，

```
 vim /etc/sysconfig/network-scripts/ifcfg-ens33
```

但是乌班图进行了更改

修改

/etc/netplan/01-network-manager-all.yaml这个文件里面的内容

```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    ens33:
      dhcp4: no
      addresses:
        - 192.168.98.131/24   # 静态IP地址，子网掩码 /24 表示 255.255.255.0
      routes:
        - to: 0.0.0.0/0        # 路由到默认网关
          via: 192.168.98.1     # 默认网关是 192.168.98.1
      nameservers:
        addresses:
          - 8.8.8.8            # 使用的 DNS 服务器 (Google DNS)
          - 8.8.4.4

```

或者是直接在图形界面的设置中，网络设置上面的小齿轮

进行设置ip 子网掩码 和网关



```
hostname
```

进行查看自己主机名

修改/etc/hostname修改自己的主机名，**重启生效**

```
source /ect/hostname
```

可以快速生效

要指定ip地址和主机名之间的关系

windows:

```
C:\Window\system32\dirvers\etc\host
```

linux:

```
/etc/hosts
```

来分别指定，其实就是一个**域名解析**

```
☁  ~  ping mengnankk
PING mengnankk.localdomain (192.168.98.1) 56(84) bytes of data.
```

hosts解析

1.浏览器先检查浏览器缓存中是不是有该域名的解析，有就直接进行ip的解析，没有找到，就检查dns检查缓存（电脑第一次访问某个网站时，浏览器或操作系统就会保存ip）

2.ipconfig /displaydns检查域名解析缓存

ipconfig /flushdns 刷新缓存

3.先检查浏览器，再检查系统的dns缓存，再去hosts检查，都没找到就会到域名dns（互联网的）进行解析。再没找到就会返回域名不存在

4.所以无痕浏览不是真的无痕

dns不是一台服务器，是**一组服务器**（集群）

5.hosts文件被修改了就会出现**dns域名劫持（dns污染）**

# ssh

ping通我们就可以用ssh连接linux主机了

因为乌班图不自动带ssh的功能

要下载

### 1. **安装 SSH 服务**

首先，确保你的 Ubuntu 系统已经安装了 SSH 服务。默认情况下，Ubuntu 并不安装 `OpenSSH` 服务。你可以通过以下步骤进行安装：

```
bash复制代码sudo apt update
sudo apt install openssh-server
```

### 2. **检查 SSH 服务是否运行**

安装完成后，确保 SSH 服务正在运行：

```
bash


复制代码
sudo systemctl status ssh
```

如果没有运行，你可以通过以下命令启动 SSH 服务：

```
bash


复制代码
sudo systemctl start ssh
```

还可以使用下面的命令设置开机自动启动 SSH 服务：

```
bash


复制代码
sudo systemctl enable ssh
```

### 3. **配置防火墙 (ufw) 来开放 SSH 端口**

Ubuntu 上通常启用了防火墙 (`ufw`)，你需要允许 SSH 流量通过防火墙：

```
bash


复制代码
sudo ufw allow ssh
```

或者你可以手动开放端口号 22（SSH 默认端口）：

```
bash


复制代码
sudo ufw allow 22
```

启用防火墙：

```
bash


复制代码
sudo ufw enable
```

最后ssh启动！
