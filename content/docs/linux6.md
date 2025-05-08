---
title: Linux zsh使用
categories:
  - 408
  
abbrlink: 2701
date: 2024.9.29
tags: 
   - Linux
   - shell
---

# zsh

## 基础配置

zsh是bash的升级版可以说是

可以使用

```
bat /etc/shells
```

来查看本系统可以支持的shell有哪些

```
☁  clash  bat /etc/shells 
───────┬────────────────────────────────────────────────────────────────────────
       │ File: /etc/shells
───────┼────────────────────────────────────────────────────────────────────────
   1   │ # /etc/shells: valid login shells
   2   │ /bin/sh
   3   │ /bin/bash
   4   │ /usr/bin/bash
   5   │ /bin/rbash
   6   │ /usr/bin/rbash
   7   │ /usr/bin/sh
   8   │ /bin/dash
   9   │ /usr/bin/dash
  10   │ /bin/zsh
  11   │ /usr/bin/zsh
```

可以发现支持哪些shell

安装zsh

```
sudo apt-fast install -y zsh
```

安装oh-my-zsh

```
sudo apt-fast install -y git
wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -
```

就会在用户下面多了

```
.oh-my-zsh
.zshrc
```

前面的是目录

后面的是配置文件

```
chsh -s /bin/zsh root
```

为root用户设定为zsh



```
alias ggpur='ggu'
alias g='git'
alias ga='git add'
alias gaa='git add --all'
alias gapa='git add --patch'
alias gau='git add --update'
alias gav='git add --verbose'
alias gwip='git add -A; git rm $(git ls-files --deleted) 2> /dev/null; git commit --no-verify --no-gpg-sign --message "--wip-- [skip ci]"'
alias gam='git am'
alias gama='git am --abort'
alias gamc='git am --continue'
alias gamscp='git am --show-current-patch'
alias gams='git am --skip'
alias gap='git apply'
alias gapt='git apply --3way'
alias gbs='git bisect'

```

与git相关的缩写

## 主题

为zsh设置主题也是很好的

我选择的是大家很多人都在用的主题

```
ZSH_THEME="powerlevel10k/powerlevel10k"
```

![链接](https://github.com/romkatv/powerlevel10k)

要让这个主题显示完全还需要下载字体

power支持的字体

Molar就可以，然后更换终端的字体即可

```
# 用法: vopen filename.html
#任务倒计时
alias timer='echo "Timer started. Stop with Ctrl-D." && date && time cat && date'
#创建目录并跳转
mcd() {
  mkdir -p "$1" && cd "$1"
}
#快速查找和杀死进程
alias psg="ps aux | grep -v grep | grep -i -e VSZ -e"
alias k9="kill -9"

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
"~/.zshrc" 142L, 4291B 
```

我的部分shell参考

# 问题解决

在打开终端的时候可能会出现问题

```
powerlevel10k] fetching gitstatusd .. |                                        
[ERROR]: gitstatus failed to initialize.

Failed to download gitstatusd-linux-x86_64 from any mirror:

  1. https://github.com/romkatv/gitstatus/releases/download/v1.5.4/gitstatusd-linux-x86_64.tar.gz
  2. https://gitee.com/romkatv/gitstatus/raw/release-v1.5.4/release/gitstatusd-linux-x86_64.tar.gz

Check your internet connection, then restart your shell
```

这是因为缺少一个组件引起的

```
wget https://github.com/romkatv/gitstatus/releases/download/v1.5.4/gitstatusd-linux-x86_64.tar.gz
```

下载该组件

解压

```
tar -xzf gitstatusd-linux-x86_64.tar.gz

```

移动

```
sudo mv gitstatusd /usr/local/bin/
```

```
sudo chmod +x /usr/local/bin/gitstatusd
```

给可执行的权限

重新启动终端即可，就会发现没有这个问题了捏