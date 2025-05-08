---
title: Linux 运维篇
categories:
  - 408
  
abbrlink: 2701
date: 2024.10.14
tags: 
   - Linux
---

# 网络运维

## 无法访问外网的域名

应该就是dns解析出了问题

我们就要在hosts解析里进行修改

```
echo "192.168.0.1 hostname" >> /etc/hosts
```

在文件中添加本机ip

```
❯ hostname
mengnankk-linux
```

配置信赖的 DNS 服务器

执行 `vi /etc/resolv.conf` ，添加以下内容：

```
nameserver 114.114.114.114
nameserver 8.8.8.8
```

一个是国内老牌的

一个是谷歌的dns解析

然后ping一下是不是能ping通

```
❯ ping mengnankk.asia
PING mengnankk.asia (76.76.21.93) 56(84) bytes of data.
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=1 ttl=128 time=95.4 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=2 ttl=128 time=121 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=3 ttl=128 time=118 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=4 ttl=128 time=106 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=5 ttl=128 time=149 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=6 ttl=128 time=97.2 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=7 ttl=128 time=125 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=8 ttl=128 time=116 ms
64 bytes from 76.76.21.93 (76.76.21.93): icmp_seq=9 ttl=128 time=103 ms
^C
--- mengnankk.asia ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 8015ms
rtt min/avg/max/mdev = 95.444/114.418/148.506/15.561 ms

```

ok了可以ping通说明我们已经配置成功了

## 网卡配置

使用root权限编辑

`/etc/sysconfig/network-scripts/ifcfg-eno16777736X` 文件

内容如下

```
TYPE=Ethernet                        # 网络类型：Ethernet以太网
BOOTPROTO=none                       # 引导协议：自动获取、static静态、none不指定
DEFROUTE=yes                         # 启动默认路由
IPV4_FAILURE_FATAL=no                # 不启用IPV4错误检测功能
IPV6INIT=yes                         # 启用IPV6协议
IPV6_AUTOCONF=yes                    # 自动配置IPV6地址
IPV6_DEFROUTE=yes                    # 启用IPV6默认路由
IPV6_FAILURE_FATAL=no                # 不启用IPV6错误检测功能
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_PRIVACY="no"

NAME=eno16777736                     # 网卡设备的别名（需要和文件名同名）
UUID=90528772-9967-46da-b401-f82b64b4acbc  # 网卡设备的UUID唯一标识号
DEVICE=eno16777736                   # 网卡的设备名称
ONBOOT=yes                           # 开机自动激活网卡
IPADDR=192.168.1.199                 # 网卡的固定IP地址
PREFIX=24                            # 子网掩码
GATEWAY=192.168.1.1                  # 默认网关IP地址
DNS1=8.8.8.8                         # DNS域名解析服务器的IP地址
```

修改完后，执行 `systemctl restart network.service` 重启网卡服务。

## 系统维护

### Linux 开机自启动脚本



**（1）在 `/etc/rc.local` 文件中添加命令**

如果不想将脚本粘来粘去，或创建链接，可以在 `/etc/rc.local` 文件中添加启动命令

1. 先修改好脚本，使其所有模块都能在任意目录启动时正常执行;
2. 再在 `/etc/rc.local` 的末尾添加一行以绝对路径启动脚本的行;

例：

执行 `vim /etc/rc.local` 命令，输入以下内容：

```
#!/bin/sh
#
# This script will be executed *after* all the other init scripts.
# You can put your own initialization stuff in here if you don't
# want to do the full Sys V style init stuff.

touch /var/lock/subsys/local
/opt/pjt_test/test.pl
```

这个感觉比较舒服

**（2）在 `/etc/rc.d/init.d` 目录下添加自启动脚本**

Linux 在 `/etc/rc.d/init.d` 下有很多的文件，每个文件都是可以看到内容的，其实都是一些 shell 脚本或者可执行二进制文件。

Linux 开机的时候，会加载运行 `/etc/rc.d/init.d` 目录下的程序，因此我们可以把想要自动运行的脚本放到这个目录下即可。系统服务的启动就是通过这种方式实现的。

**(3)现在新使用systemctl服务来添加自启动的脚本**

先保存好要使用的脚本，然后确定对脚本有可执行权限

**创建一个 `systemd` 服务文件**： 在 `/etc/systemd/system/` 目录下创建一个服务文件

```
❯ sudo nano /etc/systemd/system/clashvpn.service
```

然后对文件进行编辑

例如

```
[Unit]
Description=Run the shell script at startup
After=network.target

[Service]
Type=simple
ExecStart=/bin/zsh /home/mengnankk/shellcoode/clash.sh

[Install]
WantedBy=multi-user.target

```

然后重新加载，设置自启动

```
❯ sudo systemctl daemon-reload

❯ sudo systemctl enable clashvpn.service
Created symlink /etc/systemd/system/multi-user.target.wants/clashvpn.service → /etc/systemd/system/clashvpn.service.
```

然后查看启动，查看当前状态

```
❯ sudo systemctl start clashvpn.service
❯ sudo systemctl status clashvpn.service
● clashvpn.service - Run the clash shell script at startup
     Loaded: loaded (/etc/systemd/system/clashvpn.service; enabled; vendor pres>
     Active: active (running) since Wed 2024-10-16 18:31:28 CST; 20s ago
   Main PID: 18080 (zsh)
      Tasks: 12 (limit: 4551)
```

ps和查看一下自启动是否成功

```
❯ ps -aux|rg clash
mengnan+   11952  0.0  0.0  13712  3840 pts/0    S+   18:24   0:00 /bin/zsh ./shclash.sh
root       18080  0.0  0.0  13708  3840 ?        Ss   18:31   0:00 /bin/zsh /home/mengnankk/shellcode/shclash.sh
mengnan+   18339  0.0  0.1   8456  5888 pts/1    S+   18:32   0:00 rg clash
❯ sudo systemctl is-enabled clashvpn.service
enabled
```

发现成功了捏

我们重新启动一下，看看是不能用上哈

# 环境配置

## maven的安装

进入官网

```
https://maven.apache.org/download.cgi
```

然后下载合适的版本

然后解压

```
❯ sudo tar -xzvf apache-maven-3.9.9-bin.tar.gz -C /opt/maven --strip-components=1
```

输入 `vim /etc/profile` ，添加环境变量如下：

```
export MAVEN_HOME=/opt/maven/apache-maven-3.9.9
export PATH=$MAVEN_HOME/bin:$PATH
```

编辑然后执行生效

```
❯ sudo vim /etc/profile
❯ source /etc/profile
```

mvn -v 查看是否安装成功

```
❯ mvn -v
Apache Maven 3.9.9 (8e8579a9e76f7d015ee5ec7bfcdc97d260186937)
Maven home: /opt/maven
Java version: 11.0.24, vendor: Ubuntu, runtime: /usr/lib/jvm/java-11-openjdk-amd64
Default locale: zh_CN, platform encoding: UTF-8
OS name: "linux", version: "6.8.0-47-generic", arch: "amd64", family: "unix"
```

## nodejs安装

执行安装脚本

```
❯ rm -rf ~/.nvm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
. ~/.nvm/nvm.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 12540  100 12540    0     0  14905      0 --:--:-- --:--:-- --:--:-- 14928
=> Downloading nvm from git to '/home/mengnankk/.nvm'
=> 正克隆到 '/home/mengnankk/.nvm'...
remote: Enumerating objects: 264, done.
remote: Counting objects: 100% (264/264), done.
remote: Compressing objects: 100% (230/230), done.
remote: Total 264 (delta 31), reused 102 (delta 24), pack-reused 0 (from 0)
接收对象中: 100% (264/264), 116.38 KiB | 384.00 KiB/s, 完成.
处理 delta 中: 100% (31/31), 完成.
注意：正在切换到 '7ad6d98cedde01809e32d56ab8ced064f6f28175'。

您正处于分离头指针状态。您可以查看、做试验性的修改及提交，并且您可以在切换
回一个分支时，丢弃在此状态下所做的提交而不对分支造成影响。

如果您想要通过创建分支来保留在此状态下所做的提交，您可以通过在 switch 命令
中添加参数 -c 来实现（现在或稍后）。例如：

  git switch -c <新分支名>

或者撤销此操作：

  git switch -

通过将配置变量 advice.detachedHead 设置为 false 来关闭此建议

=> Compressing and cleaning up git repository

=> Appending nvm source string to /home/mengnankk/.zshrc
=> Appending bash_completion source string to /home/mengnankk/.zshrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
❯  nvm --version
0.33.8
```

然后查看是否安装成功

```
nvm install 8.9.4
nvm use 8.9.4
```

使用指令安装指定版本

```
❯ nvm install 8.9.4
nvm use 8.9.4
Version '8.9.4' not found - try `nvm ls-remote` to browse available versions.
N/A: version "8.9.4 -> N/A" is not yet installed.

You need to run "nvm install 8.9.4" to install it before using it.
```

但我折腾了半天，还是不能使用npm来下载

所以就是用

```
    ~  sudo apt install -y nodejs   
```

这个来直接下载了

检查一下版本

```
npm -v

v3.3.1
4.6.1
```

ok完成
