---
title: Linux基础3
categories:
  - 408
  
abbrlink: 2701
date: 2024.9.22
tags: 
   - Linux
---

# 进程

每一个运行的程序都是一个进程，每个进程都会分配一个pid号（进程号）

在windows中一个程序有多个线程

**程序run到内存里面就是一个进程**

每个进程都可能有两种方式：

**前台**和**后台**

后台的进程一般是守护进行，一般的系统服务就是以后台进程的方式进行的

比如**mysql**进程

**ps**命令用来查看系统中有哪些进程在运行

- -a 显示所以
- -u以用户的形式
- -x显示后台进程运行的参数

ps命令会显示

pid tty time cmd 四种信息选项

tty是终端机号，终端名称

time是 进程所消耗cpu的时间

cmd 是正在执行的命令或进程的名字，执行指令

**一般来说组合使用******再加上|more分页查看**

我的部分进程

```
root      106193  0.0  0.0      0     0 ?        I    18:27   0:00 [kworker/u259
root      106371  1.8  0.2 302928  9088 ?        Ssl  18:27   0:00 /usr/libexec/
root      106372  0.0  0.0      0     0 ?        I    18:27   0:00 [kworker/5:1]
root      106385  1.5  0.1  14836  6912 ?        Ss   18:27   0:00 /lib/systemd/
root      106386  0.0  0.0      0     0 ?        I    18:27   0:00 [kworker/2:2]
root      106387  0.0  0.0      0     0 ?        I    18:27   0:00 [kworker/6:1]
mengnan+  106388  7.3  1.6 3170264 65356 ?       Sl   18:27   0:00 gjs /usr/shar
root      106408  0.0  0.0      0     0 ?        I<   18:27   0:00 [kworker/u262
root      106409  0.0  0.0      0     0 ?        I<   18:27   0:00 [kworker/u262
root      106412  0.0  0.0      0     0 ?        I<   18:27   0:00 [kworker/u265
root      106413  0.0  0.0      0     0 ?        I<   18:27   0:00 [kworker/u265
root      106414  0.0  0.0      0     0 ?        I<   18:27   0:00 [kworker/u265
root      106415  0.0  0.0      0     0 ?        I<   18:27   0:00 [kworker/u265
mengnan+  106609  0.0  0.0  16044  3584 pts/4    R+   18:27   0:00 ps -uxa

```

vsz占用虚拟内存的大小

%mem占用实际内存百分比

rss占用物理内存的大小

stat r- 运行 d-短期 **z-是停用**要删除

S-睡眠 s-会话先行 t-是跟踪或者停止

## 父子进程

```
ps -ef
```

显示当前所有进程

ppid是父进程的Id

```
ps -ef|grep sshd
```

return

```
☁  ~  ps -ef|grep sshd
root        1152       1  0 13:30 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root       28205    1152  0 13:44 ?        00:00:00 sshd: mengnankk [priv]
mengnan+   28287   28205  0 13:44 ?        00:00:00 sshd: mengnankk@pts/1
root       30911    1152  0 14:14 ?        00:00:00 sshd: mengnankk [priv]
mengnan+   30949   30911  0 14:14 ?        00:00:00 sshd: mengnankk@pts/3
root       33487    1152  0 14:34 ?        00:00:00 sshd: mengnankk [priv]
mengnan+   33525   33487  0 14:34 ?        00:00:00 sshd: mengnankk@pts/4
mengnan+  120738   33526  0 18:56 pts/4    00:00:00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn --exclude-dir=.idea --exclude-dir=.tox sshd

```

父进程可以产生子进程

```
☁  ~  ps -ef|more     
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 13:30 ?        00:00:19 /sbin/init splash
root           2       0  0 13:30 ?        00:00:00 [kthreadd]
root           3       2  0 13:30 ?        00:00:00 [pool_workqueue_release]
root           4       2  0 13:30 ?        00:00:00 [kworker/R-rcu_g]
root           5       2  0 13:30 ?        00:00:00 [kworker/R-rcu_p]
root           6       2  0 13:30 ?        00:00:00 [kworker/R-slub_]
root           7       2  0 13:30 ?        00:00:00 [kworker/R-netns]
root 
```

为1的是最先出现的进程

```
top
```

top命令动态查找进程

按/进入搜索模式

或者是grep进行过滤

```
top -b -n 1 | grep python
```

这表示只显示一次

## 杀死进程

进程不想让他继续运行

```
kill -9 pid
```

```
kill -9 进程名称
```

强制立刻停止（杀死进程），不让他屏蔽

```
killall 进程名称
```

杀死进程，这里支持统配符

踢掉用户：

```
kill -9 pid
```

启动服务

```
systemctl start sshd.service
```

重启之后进程号可能会变化

```
pstree
```

更加清晰

-p/-u显示选项

我的一部分

```
├─systemd-hostnam(126605)
           ├─systemd-journal(401)
           ├─systemd-logind(1010)
           ├─systemd-oomd(740,systemd-oom)
           ├─systemd-resolve(748,systemd-resolve)
           ├─systemd-udevd(457)
           ├─udisksd(1012)─┬─{udisksd}(1034)
           │               ├─{udisksd}(1040)
           │               ├─{udisksd}(1088)
           │               └─{udisksd}(1108)
           ├─unattended-upgr(1130)───{unattended-upgr}(1220)
           ├─upowerd(2899)─┬─{upowerd}(2901)
           │               └─{upowerd}(2902)
           ├─vmtoolsd(796)─┬─{vmtoolsd}(1101)
           │               ├─{vmtoolsd}(1102)
           │               └─{vmtoolsd}(1118)
           ├─vmware-vmblock-(432)─┬─{vmware-vmblock-}(433)
           │                      └─{vmware-vmblock-}(434)
           └─wpa_supplicant(1013)

```

当杀死服务后可以使用nice命令进行启动

使用nice命令启动之后会有一个niceness值（也就是程序的优先级）

```
nice vi &
```

设置vi的niceness值

```
sudo nice -n -5 vi &
```

为-5，其值越小代表优先级成都越高

```
ps -o pid,comm,nice -C vi
```

确认vi的优先级

调小10

```
sudo renice -n -10 -p pid
```

调大10

```
renice +10 -p pid
```

jobs查看任务的状态

```
☁  ~  jobs
[1]  + suspended (tty output)  nice vi

```

fg 移动到前台，bg移动到后台

```
nohup vi &
```

让vi脱离终端

# 服务

linux中的服务也叫守护程序，很多守护程序，开机的时候自动启动，关机的时候自动关闭

init为第一个进程 pid=1

自动监护所有的孤儿进程（无父进程的）

还有一部分使用systemd 其中pid =1

负责启动其他的程序，有许多基础的功能

## service

service在/etc/init.d/文件中寻找脚本运行，一般里面都是系统脚本

```
service [name] start
service [name] stop
service [name] restart
service [name] status
```

service --status-all

查看当前运行的全部服务

```
☁  ~  service --status-all
 [ + ]  acpid
 [ - ]  alsa-utils
 [ - ]  anacron
 [ + ]  apparmor
 [ + ]  apport
 [ + ]  atd
 [ + ]  avahi-daemon
 [ + ]  bluetooth
 [ - ]  console-setup.sh
 [ + ]  cron
 [ + ]  cups
 [ + ]  cups-browsed
 [ + ]  dbus
 [ + ]  docker
 [ + ]  gdm3
 [ - ]  grub-common
 [ - ]  hwclock.sh
 [ + ]  irqbalance
 [ + ]  kerneloops
 [ - ]  keyboard-setup.sh
 [ + ]  kmod
 [ + ]  mysql
 [ + ]  nmbd
 [ + ]  ntp
 [ + ]  open-vm-tools
 [ + ]  openvpn
 [ - ]  plymouth
 [ + ]  plymouth-log
 [ + ]  preload
 [ + ]  procps
 [ - ]  pulseaudio-enable-autospawn
 [ - ]  redis-server
 [ - ]  rsync
 [ - ]  samba-ad-dc
 [ - ]  saned
 [ + ]  smbd
 [ - ]  speech-dispatcher
 [ - ]  spice-vdagent
 [ + ]  ssh
 [ + ]  udev
 [ + ]  ufw
 [ + ]  unattended-upgrades
 [ - ]  uuidd
 [ - ]  whoopsie
 [ - ]  x11-common
```

其中'+'代表运行，“-“代表停止 ”？“代表没有

自定义的服务还是放在/etc/init.d/文件下

这样就可以运行服务



## systemctl

```
systemctl status

```

```
☁  ~  systemctl status
● mengnankk-linux
    State: degraded
     Jobs: 0 queued
   Failed: 2 units
    Since: Mon 2024-09-23 18:03:39 CST; 1h 53min ago
   CGroup: /
           ├─user.slice 
           │ └─user-1000.slice 
           │   ├─user@1000.service 
           │   │ ├─session.slice 
           │   │ │ ├─org.gnome.SettingsDaemon.MediaKeys.service 
           │   │ │ │ └─4094 /usr/libexec/gsd-media-keys
           │   │ │ ├─org.gnome.SettingsDaemon.Smartcard.service 
           │   │ │ │ └─4118 /usr/libexec/gsd-smartcard
           │   │ │ ├─org.gnome.SettingsDaemon.Datetime.service 
           │   │ │ │ └─4082 /usr/libexec/gsd-datetime
           │   │ │ ├─xdg-document-portal.service 
           │   │ │ │ ├─3812 /usr/libexec/xdg-document-portal
           │   │ │ │ └─3822 fusermount3 -o rw,nosuid,nodev,fsname=portal,auto_u>
           │   │ │ ├─org.gnome.SettingsDaemon.Housekeeping.service 
           │   │ │ │ └─4088 /usr/libexec/gsd-housekeeping
           │   │ │ ├─xdg-desktop-portal.service 
           │   │ │ │ └─4389 /usr/libexec/xdg-desktop-portal
lines 1-23

```

```
systemctl start []
systemctl stop []
systemctl enable []#开机启动
systemctl disable []#解除开机启动
systemctl daemon-reload#重新载入
systemctl is-enabled 是否是自启动的
```

```
☁  ~  systemctl is-enabled sshd
alias
```

说明是自启动的

systemd使用单元unit进行控制

unit是由ini纯文本文件进行配置的

需要在/etc/systemd/system文件夹下有相对应的.service文件

在更改文件之后需要重新载入，然后重启

```
systemctl daemon-reload && systemctl start []
```

目标文件以.target结尾，其作用是

### `.target` 的作用：

1. **分组和依赖管理**：`.target` 可以将多个服务、挂载点、设备、socket 等组合在一起。当启动或停止某个 `.target` 时，systemd 会自动启动或停止与之关联的所有服务。
2. **启动序列控制**：通过 `.target` 文件，systemd 可以按照依赖关系来决定启动服务的顺序。某些服务会依赖于特定的 `.target` 才能启动。
3. **系统状态定义**：`.target` 用于定义系统的特定状态，例如用户模式、多用户模式、图形界面模式等。

```
systemctl list-units --type=target
```

查看当前的target状态

查看详细信息

```
systemctl status <target-name>.target
```

在/usr/lib/system/system文件下

## 防火墙

```
firewall-cmd --permanent --add-port=port/协议
```

打开端口（永久打开）】

```
 firewall-cmd --reload
```

使用这个重启生效

```
firewall-cmd --permanent --remove-port=port/协议
```

关闭端口

```
firewall-cmd --query-port=端口/协议
```

查看端口是否开放

## 动态监控

top 

- -d 秒数（默认是3秒）

- -i 不显示闲置或者僵尸进程

- -p 仅监控某个进程

可以使用进阶版htop

```
☁  ~  htop -
--delay      -d              -- update frequency                              
--help       -h              -- display usage information                     
--no-colour  --no-color  -C  -- monochrome mode                               
--pid        -p              -- show given pids                               
--sort-key   -s              -- sort by key                                   
--tree       -t              -- show tree view of processes                   
--user       -u              -- show processes of user                        
--version    -v              -- display version information
```

选项说明

**PID**：进程 ID，系统分配给每个进程的唯一标识。

**USER**：进程所属用户。

**PRI**：优先级，数值越小优先级越高。

**NI**：用户定义的优先级值，数值越小优先级越高。

**VIRT**：进程使用的虚拟内存总量。

**RES**：进程使用的实际物理内存量。

**SHR**：进程共享的内存量。

**S**：进程状态（如 S=睡眠，R=运行，Z=僵尸等）。

**%CPU**：进程使用的 CPU 百分比。

**%MEM**：进程使用的物理内存百分比。

**TIME+**：进程累计使用的 CPU 时间。

**COMMAND**：运行的命令或程序名称。

## 监控网络状态

netstat

- -an 按第一顺
- -p显示哪个进程

```
udp        0      0 192.168.98.131:68       192.168.98.254:67       ESTABLISHED

```

这个代表windows主机和linux虚拟机以udp协议连接，其实就是虚拟机的连接

windows走14498端口，linux开22端口



ss是netstat的升级版

1. **查看所有连接**：
   ```bash
   ss -a
   ```

2. **查看监听的套接字**：
   ```bash
   ss -l
   ```

3. **查看 TCP 连接**：
   ```bash
   ss -t
   ```

4. **查看 UDP 连接**：
   ```bash
   ss -u
   ```

5. **显示详细信息**：
   ```bash
   ss -s
   ```

使用 `ss` 可以更高效地获取网络连接信息，是 `netstat` 的一个很好的替代方案。

```
☁  ~  sudo netstat -p |rg sshd      
[sudo] mengnankk 的密码： 
unix  3      [ ]         流        已连接     11937    1064/sshd: /usr/sbi 
```

使用实例

**在使用中网络变慢，或者端口过大的时候就要注意了哈**

ping 命令用来检测连接是否畅通的

# 软件包管理

## dpkg

- -l 列出所有包
- -s pack_name 查看特定包的信息
- -L pack_name 列出特定包的信息
- -I pack_name 查看包的依赖关系

```
gs
ii  gir1.2-gtksource-4:amd64                   4.8.3-1                                           amd64        gir files for the GTK+ syntax highlighting widget
ii  gir1.2-gudev-1.0:amd64                     1:237-2build1                                     amd64        libgudev-1.0 introspection data
ii  gir1.2-gweather-3.0:amd64                  40.0-5build1                                      amd64        GObject introspection data for the GWeather library
ii  gir1.2-handy-1:amd64                       1.6.1-1                                           amd64        GObject introspection files for libhandy
ii  gir1.2-harfbuzz-0.0:amd64                  2.7.4-1ubuntu3.1                                  amd64        OpenType text shaping engine (GObject introspection data)

```

```
sudo dpkg -i package.deb
```

安装包

```
sudo dpkg -r package_name
```

卸载包

```
sudo dpkg --purge package_name
```

完全卸载

```
sudo apt-get install -f
```

修复缺失的依赖

```
dpkg -I package.deb
```

查看包的依赖关系

**一般需要先瞎咋.deb文件，然后使用dpkg进行安装**

## apt

- 列出所有可更新的软件清单命令：

  ```
  sudo apt update
  ```

  

- 升级软件包：

  ```
  sudo apt upgrade
  ```

  列出可更新的软件包及版本信息：

  ```
  apt list --upgradable
  ```

  升级软件包，升级前先删除需要更新软件包：

  ```
  sudo apt full-upgrade
  ```

  

- 安装指定的软件命令：

  ```
  sudo apt install <package_name>
  ```

  安装多个软件包：

  ```
  sudo apt install <package_1> <package_2> <package_3>
  ```

  

- 更新指定的软件命令：

  ```
  sudo apt update <package_name>
  ```

  

- 显示软件包具体信息,例如：版本号，安装大小，依赖关系等等：

  ```
  sudo apt show <package_name>
  
  ```

  删除软件包命令：

  ```
  sudo apt remove <package_name>
  ```

  

- 清理不再使用的依赖和库文件: 

  ```
  sudo apt autoremove
  ```

  

- 移除软件包及配置文件: 

  ```
  sudo apt purge <package_name>
  ```

  

- 查找软件包命令： 

  ```
  sudo apt search <keyword>
  ```

  

- 列出所有已安装的包：

  ```
  apt list --installed
  ```

  

- 列出所有已安装的包的版本信息：

  ```
  apt list --all-versions
  ```

  

## 解压包安装

```
./configure --help
```

查看安装的帮助

```
./configure --prefix
```

选择安装路径可以，当不需要这个软件的时候直接删除就可

安装实例：

首先先下载某个文件的压缩包

进行解压

```
tar -jxvf fffff.tar.bz2
```

然后

```
./configure --prefix=/opt/fictx
```

```
make
```

```
make install
```

即可

源代码软件资源需要两个条件

- 源代码是可见的
- 宽松的许可证书

这种软件是可以按照约定的规则来进行修改的

## 问题解决

```
☁  ~  sudo apt update                             
[sudo] mengnankk 的密码： 
命中:1 http://mirrors.aliyun.com/docker-ce/linux/ubuntu jammy InRelease
命中:2 https://mirrors.aliyun.com/ubuntu jammy InRelease                       
命中:3 https://mirrors.aliyun.com/ubuntu jammy-security InRelease              
命中:4 https://mirrors.aliyun.com/ubuntu jammy-updates InRelease               
命中:5 https://mirrors.aliyun.com/ubuntu jammy-backports InRelease             
命中:6 https://dl.google.com/linux/chrome/deb stable InRelease                 
命中:7 https://packages.microsoft.com/repos/code stable InRelease              
命中:8 https://download.sublimetext.com apt/stable/ InRelease                  
命中:9 https://ppa.launchpadcontent.net/fossfreedom/indicator-sysmonitor/ubuntu jammy InRelease
命中:10 https://ppa.launchpadcontent.net/linrunner/tlp/ubuntu jammy InRelease
忽略:11 https://ppa.launchpadcontent.net/noobslab/macbuntu/ubuntu jammy InRelease
命中:12 https://ppa.launchpadcontent.net/obsproject/obs-studio/ubuntu jammy InRelease
命中:13 https://ppa.launchpadcontent.net/papirus/papirus/ubuntu jammy InRelease
错误:14 https://ppa.launchpadcontent.net/noobslab/macbuntu/ubuntu jammy Release
  404  Not Found [IP: 185.125.190.80 443]
正在读取软件包列表... 完成
E: 仓库 “https://ppa.launchpadcontent.net/noobslab/macbuntu/ubuntu jammy Release” 没有 Release 文件。
N: 无法安全地用该源进行更新，所以默认禁用该源。
N: 参见 apt-secure(8) 手册以了解仓库创建和用户配置方面的细节。
```

apt出现这个问题，说明其源已经失效，这个时候就需要去/etc/apt/sources.list去更新源，然后在软件和更新界面将失效的源给删除

```
☁  ~  sudo apt update
命中:1 http://mirrors.aliyun.com/docker-ce/linux/ubuntu jammy InRelease
命中:2 https://mirrors.aliyun.com/ubuntu jammy InRelease                       
命中:3 https://mirrors.aliyun.com/ubuntu jammy-security InRelease              
命中:4 https://mirrors.aliyun.com/ubuntu jammy-updates InRelease               
命中:5 https://dl.google.com/linux/chrome/deb stable InRelease                 
命中:6 https://mirrors.aliyun.com/ubuntu jammy-backports InRelease             
命中:7 https://packages.microsoft.com/repos/code stable InRelease              
命中:8 https://download.sublimetext.com apt/stable/ InRelease                  
命中:9 https://ppa.launchpadcontent.net/fossfreedom/indicator-sysmonitor/ubuntu jammy InRelease
命中:10 https://ppa.launchpadcontent.net/linrunner/tlp/ubuntu jammy InRelease
命中:11 https://ppa.launchpadcontent.net/obsproject/obs-studio/ubuntu jammy InRelease
命中:12 https://ppa.launchpadcontent.net/papirus/papirus/ubuntu jammy InRelease
正在读取软件包列表... 完成
正在分析软件包的依赖关系树... 完成
正在读取状态信息... 完成                 
有 15 个软件包可以升级。请执行 ‘apt list --upgradable’ 来查看它们。

```

解决完成！

具体的sources.list的内容可以去**阿里云的镜像平台**进行寻找
