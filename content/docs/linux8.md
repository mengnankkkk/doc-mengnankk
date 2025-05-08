---
title: Linux 备份恢复篇
categories:
  - 408
  
abbrlink: 2701
date: 2024.10.11
tags: 
   - Linux
---

# 备份和恢复需要的工具

在使用虚拟机的时候我们可以使用快照进行恢复，但是在实体机的时候，我们不能使用快照

那我们就要掌握备份恢复的技术操作

**dump**指令和**restore**指令

```
sudo apt-fast install dump
```

```
sudo apt-fast install restorecond   
```

# dump

```
dump [options] file
```

dump指令的基本语法

选项解析：

- -0~-9代表存储的级别，0代表完全备份，1-9代表增量备份（增量备份就是备份上次备份修改或者增加的备份）
- -f 指定备份的文件或者设备，不指定默认使用磁带
- -u 显示备份更新的时间 在/etc/dumpupdates文件夹内
- -v 显示详细的信息
- -b 修改数据块的大小，默认是kb
- -s 磁带的大小
- -j 使用压缩

完全备份整个文件系统

```
dump -0uf /backup/full_backup.dump /dev/sda1
```

增量备份

```
dump -1uf /backup/full1_backup.dump /dev/sda1
```

不仅仅可以备份整个文件，还可以备份某个文件夹或者是文件

# restore

restore的基本语法

```
restore [options] file
```

详细的选项：

- -i 进入交互模式
- -r 完全恢复
- -R 恢复增量备份
- -t 显示详细信息
- -C 切换指定的目录
- -f 指定备份的文件

恢复完全

```
restore -rf /backup/full_backup.dump
```

即可

# 使用守则

为了保证数据的安全，尽量使用定时备份

```
crontab -e 
```

比如这样备份

```
0 2 * * * dump -1uf /backup/incremental_backup.dump /dev/sda1
```

在使用 `dump` 进行备份时，确保文件系统是处于一致的状态非常重要。可以使用以下几种方法：

- **单用户模式**：在备份之前进入单用户模式，以确保系统不会发生并发修改。
- **只读挂载**：将文件系统挂载为只读，防止在备份期间进行写操作。
- 在恢复时，如果你有增量备份和完全备份，恢复顺序是：
  1. 先恢复最新的完全备份。
  2. 依次应用所有增量备份（按顺序从低到高的增量级别恢复）。

# 压缩备份

使用 `-j` 选项（启用压缩），可以减少备份文件的大小。你还可以结合 Linux 的压缩工具进行备份：

```
dump -0uf - /dev/sda1 | gzip > /backup/full_backup.gz
```

恢复时，使用解压命令：

```
gunzip -c /backup/full_backup.gz | restore -rf -
```

# 跨设备备份

### 5. **跨设备备份**

如果你需要将备份存储在远程服务器上，可以使用 `ssh` 和 `dump` 结合将备份传输到远程设备：

```
dump -0uf - /dev/sda1 | ssh user@remote_server "cat > /remote_backup/full_backup.dump"
```

恢复时，同样可以通过 SSH 进行数据传输：

```
ssh user@remote_server "cat /remote_backup/full_backup.dump" | restore -rf -
```