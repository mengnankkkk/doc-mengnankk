---
title: 实用shell脚本分享
categories:
  - 408
  
abbrlink: 2701
date: 2024.10.3
tags: 
   - Linux
   - shell
---

# 1.批量修改文件的前缀和后缀

```
#!/bin/zsh
echo "请输入重命名文件夹所在的目录"
read   dir

if [ !-d "$dir" ]
then
	echo "目录不存在"
	exit 1
fi
echo "前缀"
read   prefix
echo "后缀"
read  suffix

cd "$dir"

for file in * 
do
	if [ -d "$file" ]
	then
		continue
	fi
	ext="${file##*.}"
	base="${file%.*}"

	new_name="${prefix}${base}${suffix:+.$suffix}"
	mv "$file" "$new_name"
	echo "重命名：$file->$new_name"
done
echo "完成!"
```

# 2.onedrive上传脚本

```
#!/bin/zsh
cd ~/app/onedrive-2.5.2
./onedrive --sync
echo "onedrive同步成功"

```

# 3.备份数据库

**年月日和时分秒之间的_不能丢**

**变量可以用{}包围，用来界定范围**

脚本编写的第一部分：

```
#!/bin/zsh
BACKUP=/data/backup/db
DATATIME=$(date +%Y-%m-%d_%H:%M:%S)
echo $DATATIME

HOST=LOCALHOST
DB_USER=root
PASSWD=admin
DB_NAME=test1

[ ! -d "${BACKUP}/${DATATIME}" ] && sudo mkdir -p "${BACKUP}/${DATATIME}"
```

```
[ ! -d "${BACKUP}/${DATATIME}" ] && sudo mkdir -p "${BACKUP}/${DATATIME}"
```

这句话用来创建目录

在这里面datatime只能被赋值一次，所以就是第一次的时间，不会存在其他的时间的

```
❯ sudo ./mysql_db_backup.sh
2024-10-04_17-28-14
mysqldump: [Warning] Using a password on the command line interface can be insecure.
2024-10-04_17-28-14/
2024-10-04_17-28-14/2024-10-04_17-28-14.sql.gz
数据库备份成功，备份文件为: /data/backup/db/2024-10-04_17-28-14/2024-10-04_17-28-14.sql.gz

```

运行成功截图，中间的提示错误为密码的保护问题，可以先暂时不用管

完整代码

```
#!/bin/zsh

# 备份路径
BACKUP=/data/backup/db

# 当前时间，替换 ":" 为合法符号
DATATIME=$(date +%Y-%m-%d_%H-%M-%S)
echo $DATATIME

# 数据库信息
HOST=LOCALHOST
DB_USER=root
PASSWD=admin
DB_NAME=test1

# 检查并创建备份目录
[ ! -d "${BACKUP}/${DATATIME}" ] && sudo mkdir -p "${BACKUP}/${DATATIME}"

# 使用 sudo 提升权限来执行备份
sudo mysqldump -u${DB_USER} -p${PASSWD} --host=${HOST} -q -R ${DB_NAME} | sudo gzip > ${BACKUP}/${DATATIME}/$DATATIME.sql.gz

cd ${BACKUP}
tar -zcvf $DATATIME.tar.gz ${DATATIME}
# 检查备份是否成功
if [ $? -eq 0 ]; then
    echo "数据库备份成功，备份文件为: ${BACKUP}/${DATATIME}/${DATATIME}.sql.gz"
else
    echo "数据库备份失败"
fi
sudo rm -rf ${BACKUP}/${DATATIME}

```

但是这样的话其实压缩包里面还有一个压缩包

但是不压缩两次的话，里面的东西会溢出来捏

这样我们还要把10天以前的文件给删除，否则会占满空间

```
#!/bin/zsh

# 备份路径
BACKUP=/data/backup/db

# 当前时间，替换 ":" 为合法符号
DATATIME=$(date +%Y-%m-%d_%H-%M-%S)
echo $DATATIME

# 数据库信息
HOST=LOCALHOST
DB_USER=root
PASSWD=admin
DB_NAME=test1

# 检查并创建备份目录#!/bin/zsh

# 备份路径
BACKUP=/data/backup/db

# 当前时间，替换 ":" 为合法符号
DATATIME=$(date +%Y-%m-%d_%H-%M-%S)
echo $DATATIME

# 数据库信息
HOST=LOCALHOST
DB_USER=root
PASSWD=admin
DB_NAME=test1

# 检查并创建备份目录
[ ! -d "${BACKUP}/${DATATIME}" ] && sudo mkdir -p "${BACKUP}/${DATATIME}"

# 使用 sudo 提升权限来执行备份
sudo mysqldump -u${DB_USER} -p${PASSWD} --host=${HOST} -q -R ${DB_NAME} | sudo gzip > ${BACKUP}/${DATATIME}/$DATATIME.sql.gz

cd ${BACKUP}
tar -zcvf $DATATIME.tar.gz ${DATATIME}
# 检查备份是否成功
if [ $? -eq 0 ]; then
    echo "数据库备份成功，备份文件为: ${BACKUP}/${DATATIME}/${DATATIME}.sql.gz"
else
    echo "数据库备份失败"
fi
sudo rm -rf ${BACKUP}/${DATATIME}

find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;
echo "win"
```

得到完整代码

return 

```
❯ sudo ./mysql_db_backup.sh
2024-10-04_17-39-22
2024-10-04_17-39-22
mysqldump: [Warning] Using a password on the command line interface can be insecure.
2024-10-04_17-39-22/
2024-10-04_17-39-22/2024-10-04_17-39-22.sql.gz
数据库备份成功，备份文件为: /data/backup/db/2024-10-04_17-39-22/2024-10-04_17-39-22.sql.gz
win
```

最后的最后我们准备进行定时任务了

使用crontab

```
❯ crontab -e
```

进入编辑

然后

```
30 2 * * * ~/shellcode/mysql_db_backup.sh
```

完成任务

在这里面最好使用绝对路径，我懒了就不该了哈哈
