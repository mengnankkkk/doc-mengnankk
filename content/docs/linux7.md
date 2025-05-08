---
title: Linux log日志篇
categories:
  - 408
  
abbrlink: 2701
date: 2024.10.5
tags: 
   - Linux
---

# 简介

是重要的系统信息文件，记录了重要的系统事件

一般保存在/var/log目录下

```
vmware-vmsvc-root.1.log
.rw-------  root              root            4.9 KB Sat Oct  5 08:53:43 2024  vmware-vmsvc-root.2.log
.rw-------  root              root            3.6 KB Fri Oct  4 21:03:05 2024  vmware-vmsvc-root.3.log
.rw-------  root              root            3.4 KB Sat Oct  5 09:15:19 2024  vmware-vmsvc-root.log
.rw-------  root              hadoop          1.1 KB Sun Sep 29 19:47:23 2024  vmware-vmtoolsd-hadoop.log
.rw-------  root              mengnankk        34 KB Sat Oct  5 09:15:36 2024  vmware-vmtoolsd-mengnankk.log
.rw-------  root              root             34 KB Sat Oct  5 09:15:18 2024  vmware-vmtoolsd-root.log
.rw-------  root              hadoop          4.7 KB Sun Sep 29 20:48:11 2024  vmware-vmusr-hadoop.log
.rw-------  root              mengnankk       118 KB Sat Oct  5 09:15:40 2024  vmware-vmusr-mengnankk.log
drwxrwxrwx  root              root            4.0 KB Mon Sep 30 21:39:40 2024  
```

## lastlog

直接使用

```
lastlog
```

查看最后登录用户的日志

```
❯ lastlog
用户名           端口     来自             最后登录时间
root             pts/6    127.0.0.1        二 9月 24 19:06:43 +0800 2024
daemon                                     **从未登录过**
bin                                        **从未登录过**
sys                                        **从未登录过**

```

因为是二进制文件，所以不要用cat打印了

直接使用指令就行

## boot.log

这里面是系统启动的一些日志

## cron

这里面是关于系统定时任务的一些日志记录

## message

系统出现故障，首先找这个日志，用来记录系统故障的日志

## secure

这里面主要是安全性问题，系统登录ssh等一些列的有关安全的问题，包括密码等

## utmp

记录当前登录用户的信息

w who等命令就是记录的他的信息

```
❯ w
 10:53:25 up  1:38,  1 user,  load average: 0.42, 0.15, 0.06
USER     TTY      来自           LOGIN@   IDLE   JCPU   PCPU WHAT
mengnank tty2     tty2             09:15    1:38m  0.03s  0.03s /usr/libexec/gn
```

# 日志管理服务

我的localhost使用的是rsyslog

```
❯ ps aux | rg "rsyslog"
syslog      1022  0.0  0.1 222404  6528 ?        Ssl  09:15   0:00 /usr/sbin/rsyslogd -n -iNONE
mengnan+   42697  0.0  0.1   8456  5888 pts/0    S+   10:58   0:00 rg rsyslog
```

使用该命令查看是否在运行

rsyslog是一个后台程序，就是一个日志

/etc/rsyslog.conf

在这个文件里记录要管理的日志有哪些

还有一些规则，日志类型等一系列的东西

**在进行这个日志管理的时候，这个服务必须在运行才可以捏**

```
❯ ps aux | rg "rsyslog"|rg -v "rg"
syslog      1017  0.0  0.1 222404  5760 ?        Ssl  17:24   0:00 /usr/sbin/rsyslogd -n -iNONE

```

反向再过滤一下，就只有我们

**rg -v 是反向匹配（挑选出前面不包含的）**

其实就相对于非

查看rsyslog服务是否是自启动的

```
❯ systemctl list-unit-files | rg "rsyslog"

rsyslog.service                                enabled         enabled
```

发现他是的捏

## 配置文件

/etc/rsyslog.conf这个文件

- debug
- info
- notice
- warning
- err
- crit
- alert
- emerg
- none

从下面上面，级别是由高到低的，记录的信息越来越多

emerg就是内核都崩溃了

none就是什么也不给记录

实例

修改配置文件

在/var/log/hsp.log下添加文件，看重启是否有信息显示

## 分类

**`auth`** (Authentication Logs):

- 文件：`/var/log/auth.log` (Debian/Ubuntu) 或 `/var/log/secure` (RHEL/CentOS)
- 作用：记录系统的认证事件，如用户登录、`sudo` 提权、SSH 登录尝试等。

**`syslog`** (System Logs):

- 文件：`/var/log/syslog` (Debian/Ubuntu) 或 `/var/log/messages` (RHEL/CentOS)
- 作用：捕捉常规系统事件，除了认证信息外，记录各种系统进程输出的消息。

**`kern`** (Kernel Logs):

- 文件：`/var/log/kern.log`
- 作用：记录内核生成的日志信息，如硬件检测、模块加载、内核异常等。

**`cron`** (Cron Jobs Logs):

- 文件：`/var/log/cron.log` (Debian/Ubuntu) 或 `/var/log/cron` (RHEL/CentOS)
- 作用：记录由 `cron` 调度的任务执行信息，包括定时任务的成功与失败。

**`boot`** (Boot Logs):

- 文件：`/var/log/boot.log`
- 作用：记录系统启动过程中的信息，帮助排查启动时发生的问题。

**`dmesg`** (Kernel Ring Buffer Logs):

- 文件：通过命令 `dmesg` 查看
- 作用：显示内核环缓冲中的信息，特别是与设备驱动、硬件相关的信息。

**`apache`/`nginx`** (Web Server Logs):

- 文件：`/var/log/apache2/access.log`, `/var/log/nginx/access.log`, `/var/log/apache2/error.log`, `/var/log/nginx/error.log`
- 作用：记录 Web 服务器的访问日志、错误日志，包含每次客户端请求的详细信息。

**`mysql`** (MySQL Logs):

- 文件：`/var/log/mysql.log` 或 `/var/log/mysqld.log`
- 作用：记录 MySQL 数据库的查询日志、错误日志以及慢查询日志。

**`mail`** (Mail Logs):

- 文件：`/var/log/mail.log`
- 作用：记录邮件服务器的发送和接收信息，帮助管理员监控邮件通信。

**`Xorg`** (X Window System Logs):

- 文件：`/var/log/Xorg.0.log`
- 作用：记录与图形界面启动相关的信息，包括图形硬件的状态、驱动加载情况等。

# 日志轮替

按照一定的策略。保留日志文件。旧的就是删除

使用的是logrotate

配置文件在/etc/logrotate.conf

是一个全局日志轮替规则，当然也可以给某个日志文件指定策略

配置文件中有**dateext**参数那么就会用日期来做日志文件的参数，只需要删除就可以

没有**dateext**参数的时候，就会自动改名

比如secure1 secure2

轮替的时候，1会变成2，2又会变成1以此轮替

**如果超过默认保存的个数，旧的就会删除掉**

**自定义的日志轮替规则是最高级的**

我们可以自定义

```
/var/log/wtemp{
monthly
create 0664 root utmp
minisize 1M
retate
}
```

也可以某个日志文件的轮替规则写到

/etc/logrotate.d目录下单独设置

```
❯ z /etc/logrotate.d
❯ ls
alternatives  cups-daemon   ppp           sane-utils           wtmp
apport        dnf           preload       speech-dispatcher
apt           dpkg          redis-server  ubuntu-pro-client
bootlog       firewalld     rsyslog       ufw
btmp          mysql-server  samba         unattended-upgrades
```

查看例子

```
❯ bat bootlog
───────┬────────────────────────────────────────────────────────────────────────
       │ File: bootlog
───────┼────────────────────────────────────────────────────────────────────────
   1   │ /var/log/boot.log
   2   │ {
   3   │     missingok
   4   │     daily
   5   │     copytruncate
   6   │     rotate 7
   7   │     notifempty
   8   │ }

```

发现了bootlog就是单独设置的捏

感觉一般写在这里是比较好的选择

### 常见的 `logrotate` 配置参数

1. **`daily` / `weekly` / `monthly` / `yearly`**

   - 说明：指定日志文件轮替的频率。
   - 示例：`daily` 表示每天轮替一次日志；`weekly` 表示每周轮替一次。

2. **`rotate <count>`**

   - 说明：保留的旧日志文件数量，超出数量的日志文件将被删除。
   - 示例：`rotate 5` 表示最多保留 5 个历史日志文件。

3. **`compress`**

   - 说明：启用压缩功能，轮替后的日志文件会使用 `gzip` 进行压缩。
   - 示例：`compress` 表示开启压缩，生成 `.gz` 文件。

4. **`compresscmd <command>`**

   - 说明：指定压缩时使用的命令，默认是 `gzip`。
   - 示例：`compresscmd bzip2` 可以指定使用 `bzip2` 进行压缩。

5. **`uncompresscmd <command>`**

   - 说明：指定解压缩时使用的命令。
   - 示例：`uncompresscmd bunzip2` 可以指定使用 `bunzip2` 进行解压缩。

6. **`delaycompress`**

   - 说明：推迟日志压缩，只有在下次轮替时才压缩之前的日志。
   - 示例：`delaycompress` 常与 `compress` 一起使用，表示日志轮替后不会立即压缩，而是等到下一次轮替时再压缩上一个日志文件。

7. **`create <mode> <owner> <group>`**

   - 说明：在日志轮替后创建新的日志文件，并指定其权限、所有者和所属组。
   - 示例：`create 644 root adm` 会创建一个权限为 `644`，所有者为 `root`，组为 `adm` 的新日志文件。

8. **`notifempty`**

   - 说明：如果日志文件为空，则不进行轮替。
   - 示例：`notifempty` 可避免对空日志文件进行轮替操作。

9. **`missingok`**

   - 说明：如果日志文件不存在，则跳过而不报错。
   - 示例：`missingok` 防止 `logrotate` 在找不到日志文件时产生错误。

10. **`size <size>`**

    - 说明：当日志文件达到指定的大小时进行轮替。
    - 示例：`size 100M` 表示日志文件达到 100MB 时轮替。

11. **`dateext`**

    - 说明：在轮替的日志文件名中添加日期后缀。
    - 示例：`dateext` 会让日志文件的后缀变为形如 `.20231005`。

12. **`dateformat <format>`**

    - 说明：自定义日期后缀的格式（与 `dateext` 一起使用）。
    - 示例：`dateformat -%Y%m%d` 可以将后缀格式设定为 `-YYYYMMDD`。

13. **`maxsize <size>`**

    - 说明：指定日志文件的最大大小。如果文件超过此大小，会强制进行轮替。
    - 示例：`maxsize 500M` 表示日志文件超过 500MB 时轮替。

14. **`postrotate` / `prerotate` / `endscript`**

    - 说明：定义轮替前（`prerotate`）或轮替后（`postrotate`）执行的命令。`endscript` 用于结束 `postrotate` 或 `prerotate` 块。

    - 示例：

      ```
      bash复制代码postrotate
          systemctl reload nginx
      endscript
      ```

      表示在日志轮替之后，重新加载 

      ```
      nginx
      ```

       服务。

15. **`copytruncate`**

    - 说明：复制当前日志文件，并截断原日志文件，适用于那些在打开日志文件时不能关闭的进程。
    - 示例：`copytruncate` 会在日志文件轮替时复制当前文件，然后将原文件截断为 0 字节。

16. **`olddir <directory>`**

    - 说明：将轮替后的日志文件移动到指定的目录中。
    - 示例：`olddir /var/log/old` 表示轮替后的旧日志文件会被移动到 `/var/log/old` 目录中。

17. **`mail <address>`**

    - 说明：当日志文件轮替后，将该日志文件通过邮件发送到指定的电子邮箱。
    - 示例：`mail admin@example.com` 会将轮替后的日志发送给 `admin@example.com`。

18. **`nomail`**

    - 说明：禁用邮件功能。
    - 示例：`nomail` 禁止发送日志文件邮件。

实例：

```
/var/log/nginx/*.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    create 640 www-data adm
    sharedscripts
    postrotate
        systemctl reload nginx > /dev/null 2>/dev/null || true
    endscript
}
```

这个配置的含义：

- 每天轮替 `/var/log/nginx/` 目录下的日志文件。
- 如果日志文件不存在，跳过轮替，不报错。
- 最多保留 14 个日志文件。
- 轮替后的日志文件进行压缩，但推迟到下一次轮替时再压缩。
- 如果日志文件为空，则不轮替。
- 轮替后创建权限为 `640`，所有者为 `www-data`，所属组为 `adm` 的新日志文件。
- 轮替后执行 `systemctl reload nginx`，重新加载 nginx 配置。

直接

```
vim hsplog
```

然后输入就可以了

## 机制

依赖于定时任务机制**crond**

在/etc/cron.daily下

```
❯ z /etc/cron.daily
❯ ls
0anacron  apt-compat        dpkg           locate     man-db  samba
apport    cracklib-runtime  google-chrome  logrotate  ntp
```

主要是执行logrotate这个

# 内存日志

是先写到内存里，没在日志文件里

重新启动的时候，内存日志会清空

journalctl

- 无参数查看全部
- -n num查看多少条
- --sine 19:00 --until 19:10:10 起始时间
- -o verbose 详细信息
- PID=1245 _COMM=sshd 查看这些

感觉直接筛选更好

```
.
10月 05 08:54:03 mengnankk-linux sshd[1270]: Server listening on :: port 22.
10月 05 09:14:54 mengnankk-linux sshd[1270]: Received signal 15; terminating.
10月 05 09:15:19 mengnankk-linux sshd[1262]: Server listening on 0.0.0.0 port 22.
10月 05 09:15:19 mengnankk-linux sshd[1262]: Server listening on :: port 22.
10月 05 17:24:44 mengnankk-linux sshd[1268]: Server listening on 0.0.0.0 port 22.
10月 05 17:24:44 mengnankk-linux sshd[1268]: Server listening on :: port 22.
10月 05 19:08:11 mengnankk-linux sshd[1268]: Received signal 15; terminating.
10月 05 19:08:33 mengnankk-linux sshd[1240]: Server listening on 0.0.0.0 port 22.
10月 05 19:08:33 mengnankk-linux sshd[1240]: Server listening on :: port 22.
10月 06 09:04:07 mengnankk-linux sshd[1300]: Server listening on 0.0.0.0 port 22.
10月 06 09:04:07 mengnankk-linux sshd[1300]: Server listening on :: port 22.

```

直接出现

over
