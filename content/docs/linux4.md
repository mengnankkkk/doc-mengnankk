---
title: Linux 现代化工具
categories:
  - 408
  
abbrlink: 2701
date: 2024.9.23
tags: 
   - Linux
---

# dust(du)

```
☁  ~  dust
Did not have permissions for all directories (add --print-errors to see errors)
101M         ┌── libhdfspp.a                         │██▓░░░░░░░░░░░░░░░░ │   7%
167M       ┌─┴ native                                │███░░░░░░░░░░░░░░░░ │  12%
167M     ┌─┴ lib                                     │███░░░░░░░░░░░░░░░░ │  12%
 99M     │     ┌── hadoop-project-dist               │██▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒░░ │   7%
 91M     │     │   ┌── apidocs                       │██▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒░░ │   6%
 92M     │     │ ┌─┴ hadoop-yarn-common              │██▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒░░ │   7%
 97M     │     │ ├── hadoop-yarn-server              │██▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒░░ │   7%
226M     │     ├─┴ hadoop-yarn                       │████▓▓▒▒▒▒▒▒▒▒▒▒▒░░ │  16%
441M     │   ┌─┴ hadoop                              │██████▒▒▒▒▒▒▒▒▒▒▒░░ │  31%
441M     │ ┌─┴ doc                                   │██████▒▒▒▒▒▒▒▒▒▒▒░░ │  31%
 77M     │ │ ┌── hdfs                                │██▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒░░ │   6%
 83M     │ │ ├── client                              │██▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒░░ │   6%
 91M     │ │ ├── common                              │██▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒░░ │   7%
136M     │ │ ├── yarn                                │██▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒░░ │  10%
296M     │ │ │   ┌── aws-java-sdk-bundle-1.12.367.jar│█████▓▓▓▓▓▓▒▒▒▒▒▒░░ │  21%
333M     │ │ │ ┌─┴ lib                               │█████▓▓▓▓▓▓▒▒▒▒▒▒░░ │  24%
336M     │ │ ├─┴ tools                               │█████▓▓▓▓▓▓▒▒▒▒▒▒░░ │  24%
752M     │ ├─┴ hadoop                                │███████████▒▒▒▒▒▒░░ │  54%
1.2G     ├─┴ share                                   │█████████████████░░ │  85%
1.3G   ┌─┴ hadoop                                    │███████████████████ │  97%
1.4G ┌─┴ .                                           │███████████████████ │ 100%
☁  ~  
```

查看目录和文件大小的使用

# duf（df）

```
☁  ~  duf
+---------------------------------------------------------------------+
| 3 local devices                                                     |
+--------------+--------+-------+--------+--------+------+------------+
| MOUNTED ON   |   SIZE |  USED |  AVAIL |  USE%  | TYPE | FILESYSTEM |
+--------------+--------+-------+--------+--------+------+------------+
| /            |  78.1G | 21.1G |  53.6G |  27.0% | ext4 | /dev/sda3  |
| /boot/efi    | 512.0M |  6.1M | 505.9M |   1.2% | vfat | /dev/sda2  |
| /var/snap/fi |  78.1G | 21.1G |  53.6G |  27.0% | ext4 | /dev/sda3  |
| refox/common |        |       |        |        |      |            |
| /host-hunspe |        |       |        |        |      |            |
| ll           |        |       |        |        |      |            |
+--------------+--------+-------+--------+--------+------+------------+
+------------------------------------------------------------------------+
| 6 special devices                                                      |
+--------------+--------+--------+--------+--------+--------+------------+
| MOUNTED ON   |   SIZE |   USED |  AVAIL |  USE%  | TYPE   | FILESYSTEM |
+--------------+--------+--------+--------+--------+--------+------------+
| /dev         |   1.9G |     0B |   1.9G |        | devtmp | udev       |
|              |        |        |        |        | fs     |            |
| /dev/shm     |   1.9G |     0B |   1.9G |        | tmpfs  | tmpfs      |
| /run         | 386.9M |   3.5M | 383.4M |   0.9% | tmpfs  | tmpfs      |
| /run/lock    |   5.0M |   4.0K |   5.0M |   0.1% | tmpfs  | tmpfs      |
| /run/snapd/n | 386.9M |   3.5M | 383.4M |   0.9% | tmpfs  | tmpfs      |
| s            |        |        |        |        |        |            |
| /run/user/10 | 386.9M | 152.0K | 386.7M |   0.0% | tmpfs  | tmpfs      |
| 00           |        |        |        |        |        |            |
+--------------+--------+--------+--------+--------+--------+------------+
```

df的平替，用来查看磁盘空间

# htop（top）

top的升级版，可以显示进程的大小等信息

# cargo

rust开发的下载工具

```
cargo install procs
```

下载procs

# procs（ps)

ps的平替

```
 18127 root            │       0.0 0.0 00:00:00 │ [kworker/0:0-events]         >
 18128 root            │       0.0 0.0 00:00:00 │ [kworker/u260:3-events_unboun>
 18130 mengnankk       │       0.0 0.4 00:00:00 │ /usr/bin/python3 /usr/bin/gno>
 18133 mengnankk       │       0.0 0.6 00:00:00 │ /usr/bin/gnome-terminal.real >
 18198 root            │       0.0 0.0 00:00:00 │ [kworker/6:1-events]         >
 18200 root            │       0.0 0.0 00:00:00 │ [kworker/5:2]                >
 18218 root            │       0.0 0.0 00:00:01 │ [kworker/4:0-events]         >
 18233 root            │       0.0 0.0 00:00:00 │ [kworker/u263:0-ttm]         >
 18240 root            │       0.0 0.0 00:00:00 │ [kworker/7:1-events]         >
 18241 root            │       0.0 0.0 00:00:00 │ [kworker/5:1-mm_percpu_wq]   >
 18248 root            │       0.0 0.0 00:00:00 │ [kworker/1:1-rcu_par_gp]     >
 18266 root            │       0.0 0.0 00:00:00 │ [kworker/0:2-rcu_par_gp]     >
 18273 mengnankk       │       0.0 1.1 00:00:01 │ gjs /usr/share/gnome-shell/ex>
 18298 root            │       0.0 0.0 00:00:00 │ [kworker/u265:1]             >
 18334 root            │       0.0 0.0 00:00:00 │ [kworker/3:1-events]         >
 18501 root            │       0.0 0.0 00:00:00 │ [kworker/u258:3]             >
 18591 root            │       0.0 0.0 00:00:00 │ [kworker/2:1-mm_percpu_wq]   >
 18828 root            │       0.0 0.0 00:00:00 │ [kworker/u259:2-events_unboun>
 18959 root            
```

按q退出

# exa(ls)

ls的平替

# z（cd)

cd的平替，但是需要在shell中写入

```
☁  ~  echo 'eval "$(zoxide init zsh)"' >> ~/.zshrc
```

然后重启资源

# httpie（curl）

可以代替curl下载很多资源

http/https使用

# hyperfine（time）

代替time进行分析

```
☁  /home  time lsdh
drwxr-xr-x root      root      4.0 KB Mon Sep  9 19:14:24 2024  .
drwxr-xr-x root      root      4.0 KB Sun Sep 22 14:05:14 2024  ..
drwxr-x--- mengnankk mengnankk 4.0 KB Tue Sep 24 16:32:17 2024  mengnankk
lsd -hal  0.04s user 0.02s system 93% cpu 0.061 total

```

# bat(cat)

代替cat进行打印

tab也是可以使用的

# ripgrep（grep)

代替grep进行过滤，过滤的条件会高亮显示

# doggo（dig）

进行dns的分析

```
☁  /home  doggo mengnankk.asia
NAME           	TYPE	CLASS	TTL	ADDRESS    	NAMESERVER    
mengnankk.asia.	A   	IN   	5s 	76.76.21.93	127.0.0.53:53
```

# lsd（ls）

更好的ls

```
☁  /home  lsdh
drwxr-xr-x root      root      4.0 KB Mon Sep  9 19:14:24 2024  .
drwxr-xr-x root      root      4.0 KB Sun Sep 22 14:05:14 2024  ..
drwxr-x--- mengnankk mengnankk 4.0 KB Tue Sep 24 16:36:38 2024  mengnank
```

