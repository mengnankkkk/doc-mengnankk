---

title: Mysql BASIC
categories:
  - sql
  
abbrlink: 2701
date: 2024.7.4
tags: 
   - mysql 

---

# 数据库

## 认识数据库

数据库（Database）是按照数据结构来组织、存储和管理数据的仓库。

每个数据库都有一个或多个不同的 API 用于创建，访问，管理，搜索和复制所保存的数据。

我们也可以将数据存储在文件中，但是在文件中读写数据速度相对较慢。

所以，现在我们使用关系型数据库管理系统（RDBMS）来存储和管理大数据量。所谓的关系型数据库，是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据。

RDBMS 即关系数据库管理系统(Relational Database Management System)的特点：

- 1.数据以表格的形式出现
- 2.每行为各种记录名称
- 3.每列为记录名称所对应的数据域
- 4.许多的行和列组成一张表单
- 5.若干的表单组成database

RDBMS术语

- **数据库:** 数据库是一些关联表的集合。
- **数据表:** 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
- **列:** 一列(数据元素) 包含了相同类型的数据, 例如邮政编码的数据。
- **行：**一行（元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
- **冗余**：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
- **主键**：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
- **外键：**外键用于关联两个表。
- **复合键**：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
- **索引：**使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
- **参照完整性:** 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

Mysql为关系型数据库，关系型数据库可以理解为表格的概念，一个关系型数据库由一个或数个表格构成

- 表头(header): 每一列的名称;
- 列(col): 具有相同数据类型的数据的集合;
- 行(row): 每一行用来描述某条记录的具体信息;
- 值(value): 行的具体信息, 每个值必须与该列的数据类型相同;
- **键(key)**: 键的值在当前列中具有唯一性。



## MySQL数据库

MySQL 是一个关系型数据库管理系统，由瑞典 MySQL AB 公司开发，目前属于 Oracle 公司。MySQL 是一种关联数据库管理系统，关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。

- MySQL 是开源的，目前隶属于 Oracle 旗下产品。
- MySQL 支持大型的数据库。可以处理拥有上千万条记录的大型数据库。
- MySQL 使用标准的 SQL 数据语言形式。
- MySQL 可以运行于多个系统上，并且支持多种语言。这些编程语言包括 C、C++、Python、Java、Perl、PHP、Eiffel、Ruby 和 Tcl 等。
- MySQL 对 PHP 有很好的支持，PHP 是很适合用于 Web 程序开发。
- MySQL 支持大型数据库，支持 5000 万条记录的数据仓库，32 位系统表文件最大可支持 4GB，64 位系统支持最大的表文件为8TB。
- MySQL 是可以定制的，采用了 GPL 协议，你可以修改源码来开发自己的 MySQL 系统。

## 安装mysql

安装的前面就不多说了直接去官网下载即可

```
mysqladmin --version
```

验证mysql的安装

```
mysqladmin  Ver 8.0.37 for Win64 on x86_64 (MySQL Community Server - GPL)
```

我的回显

```
net start mysql
```

启动mysql服务器

```
net stop mysql
```

关闭mysql

在 MySQL 中，mysql.user 表存储了关于用户账户的信息，包括权限和身份验证方式，以下是 **mysql.user** 表中的常见字段列表及其含义：

- `Host`: 用户连接的来源主机。
- `User`: 用户名。
- `Password`: 加密后的密码。
- `Select_priv`: 是否允许用户执行 SELECT 操作。
- `Insert_priv`: 是否允许用户执行 INSERT 操作。
- `Update_priv`: 是否允许用户执行 UPDATE 操作。
- `Delete_priv`: 是否允许用户执行 DELETE 操作。
- `Create_priv`: 是否允许用户创建新表或数据库。
- `Drop_priv`: 是否允许用户删除表或数据库。
- `Reload_priv`: 是否允许用户执行 FLUSH 操作。
- `Shutdown_priv`: 是否允许用户执行 SHUTDOWN 操作。
- `Process_priv`: 是否允许用户查看其他用户的进程。
- `File_priv`: 是否允许用户执行文件操作（如 LOAD DATA INFILE）。
- `Grant_priv`: 是否允许用户授予或撤销其他用户的权限。
- `References_priv`: 是否允许用户创建外键约束。
- `Index_priv`: 是否允许用户创建或删除索引。
- `Alter_priv`: 是否允许用户执行 ALTER TABLE 操作。
- `Show_db_priv`: 是否允许用户执行 SHOW DATABASES 操作。
- `Super_priv`: 是否允许用户执行超级权限的操作。
- `Create_tmp_table_priv`: 是否允许用户创建临时表。
- `Lock_tables_priv`: 是否允许用户锁定表。
- `Execute_priv`: 是否允许用户执行存储过程和函数。
- `Repl_slave_priv`: 是否允许用户作为复制从库。
- `Repl_client_priv`: 是否允许用户作为复制客户端。
- `Create_view_priv`: 是否允许用户创建视图。
- `Show_view_priv`: 是否允许用户执行 SHOW CREATE VIEW 操作。
- `Create_routine_priv`: 是否允许用户创建存储过程和函数。
- `Alter_routine_priv`: 是否允许用户修改存储过程和函数。
- `Create_user_priv`: 是否允许用户创建、删除和重命名用户。
- `Event_priv`: 是否允许用户创建、修改、删除事件。
- `Trigger_priv`: 是否允许用户创建、修改、删除触发器。
- `Create_tablespace_priv`: 是否允许用户创建和删除表空间。
- `ssl_type`: SSL 类型。
- `ssl_cipher`: SSL 密码。
- `x509_issuer`: X.509 证书颁发者。
- `x509_subject`: X.509 证书主题。
- `max_questions`: 用户可以执行的最大查询数量。
- `max_updates`: 用户可以执行的最大更新数量。
- `max_connections`: 用户可以同时打开的最大连接数。
- `max_user_connections`: 用户可以同时打开的最大用户连接数。

## /etc/my.cnf 文件配置

/etc/my.cnf 文件是 MySQL 配置文件，用于配置 MySQL 服务器的各种参数和选项。

一般情况下，你不需要修改该配置文件，该文件默认配置如下：

```
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

[mysql.server]
user=mysql
basedir=/var/lib

[safe_mysqld]
err-log=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

```

### 1. 基本设置

- `basedir`: MySQL 服务器的基本安装目录。
- `datadir`: 存储 MySQL 数据文件的位置。
- `socket`: MySQL 服务器的 Unix 套接字文件路径。
- `pid-file`: 存储当前运行的 MySQL 服务器进程 ID 的文件路径。
- `port`: MySQL 服务器监听的端口号，默认是 3306。

### 2. 服务器选项

- `bind-address`: 指定 MySQL 服务器监听的 IP 地址，可以是 IP 地址或主机名。
- `server-id`: 在复制配置中，为每个 MySQL 服务器设置一个唯一的标识符。
- `default-storage-engine`: 默认的存储引擎，例如 InnoDB 或 MyISAM。
- `max_connections`: 服务器可以同时维持的最大连接数。
- `thread_cache_size`: 线程缓存的大小，用于提高新连接的启动速度。
- `query_cache_size`: 查询缓存的大小，用于提高相同查询的效率。
- `default-character-set`: 默认的字符集。
- `collation-server`: 服务器的默认排序规则。

### 3. 性能调优

- `innodb_buffer_pool_size`: InnoDB 存储引擎的缓冲池大小，这是 InnoDB 性能调优中最重要的参数之一。
- `key_buffer_size`: MyISAM 存储引擎的键缓冲区大小。
- `table_open_cache`: 可以同时打开的表的缓存数量。
- `thread_concurrency`: 允许同时运行的线程数。

### 4. 安全设置

- `skip-networking`: 禁止 MySQL 服务器监听网络连接，仅允许本地连接。
- `skip-grant-tables`: 以无需密码的方式启动 MySQL 服务器，通常用于恢复忘记的 root 密码，但这是一个安全风险。
- `auth_native_password=1`: 启用 MySQL 5.7 及以上版本的原生密码认证。

### 5. 日志设置

- `log_error`: 错误日志文件的路径。
- `general_log`: 记录所有客户端连接和查询的日志。
- `slow_query_log`: 记录执行时间超过特定阈值的慢查询。
- `log_queries_not_using_indexes`: 记录未使用索引的查询。

### 6. 复制设置

- `master_host` 和 `master_user`: 主服务器的地址和复制用户。
- `master_password`: 复制用户的密码。
- `master_log_file` 和 `master_log_pos`: 用于复制的日志文件和位置。

### 7.管理MySQL的命令

**USE \*数据库名\***

选择要操作的Mysql数据库，使用该命令后所有Mysql命令都只针对该数据库。

```
use RUNOOB
```

**SHOW DATABASES:**

列出 MySQL 数据库管理系统的数据库列表。

**SHOW TABLES:**

显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库。

**SHOW COLUMNS FROM \*数据表\*:**

显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。

**SHOW INDEX FROM \*数据表\***

显示数据表的详细索引信息，包括PRIMARY KEY（主键）。

**SHOW TABLE STATUS [FROM db_name] [LIKE 'pattern'] \G:**





### 8.PHP脚本连接数据库

```php
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('Could not connect: ' . mysqli_error());
}
echo '数据库连接成功！';
mysqli_close($conn);
?>
```

## 数据库的应用（基于命令）

### 数据库基本操作

#### 创建数据库

我们可以在登陆 MySQL 服务后，使用 **create** 命令创建数据库，语法如下:

```
CREATE DATABASE 数据库名;
```

如果你希望在创建数据库时指定一些选项，可以使用 CREATE DATABASE 语句的其他参数，例如，你可以指定字符集和排序规则：

```
CREATE DATABASE mydatabase
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_general_ci;
```

如果数据库已经存在，执行 CREATE DATABASE 将导致错误。

为了避免这种情况，你可以在 CREATE DATABASE 语句中添加 IF NOT EXISTS 子句：

```
CREATE DATABASE IF NOT EXISTS mydatabase;
```

以下是使用 mysqladmin 创建数据库的基本语法：

```
mysqladmin -u your_username -p create your_database
```

在navicat中直接通过ui界面进行创建链接创建数据库

或者是在在 Navicat 中执行 SQL 命令：

1. 打开 Navicat 并连接到你的 MySQL 服务器。
2. 在左侧的连接列表中找到你的 MySQL 服务器，右键点击连接，然后选择“新建查询”。
3. 在查询编辑器中输入以下 SQL 命令来创建数据库：

```
CREATE DATABASE your_database;
```

如果你希望在创建数据库时指定字符集和排序规则，可以使用 **-default-character-set** 和 **-default-collation** 参数：

```
mysqladmin -u your_username -p create your_database \
  --default-character-set=utf8mb4 \
  --default-collation=utf8mb4_general_ci
```

php脚本

```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
  die('连接错误: ' . mysqli_error($conn));
}
echo '连接成功<br />';
$sql = 'CREATE DATABASE RUNOOB';
$retval = mysqli_query($conn,$sql );
if(! $retval )
{
    die('创建数据库失败: ' . mysqli_error($conn));
}
echo "数据库 RUNOOB 创建成功\n";
mysqli_close($conn);
?>
```

#### 删除数据库

```
DROP DATABASE <database_name>;        -- 直接删除数据库，不检查是否存在
或
DROP DATABASE [IF EXISTS] <database_name>;

```

- `IF EXISTS` 是一个可选的子句，表示如果数据库存在才执行删除操作，避免因为数据库不存在而引发错误。

以下是使用 mysqladmin 删除数据库的命令：



```
mysqladmin -u your_username -p drop your_database
```

##### 使用 PHP 脚本删除数据库

```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
echo '连接成功<br />';
$sql = 'DROP DATABASE RUNOOB';
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('删除数据库失败: ' . mysqli_error($conn));
}
echo "数据库 RUNOOB 删除成功\n";
mysqli_close($conn);
?>
```

#### 选择数据库

```
USE database_name;
```

mysqlamin:

```
mysql -u your_username -p -D your_database
```

php脚本

```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
echo '连接成功';
mysqli_select_db($conn, 'RUNOOB' );
mysqli_close($conn);
?>
```

## 数据类型

MySQL 支持所有标准 SQL 数值数据类型。

这些类型包括严格数值数据类型(INTEGER、SMALLINT、DECIMAL 和 NUMERIC)，以及近似数值数据类型(FLOAT、REAL 和 DOUBLE PRECISION)。

关键字INT是INTEGER的同义词，关键字DEC是DECIMAL的同义词。

BIT数据类型保存位字段值，并且支持 MyISAM、MEMORY、InnoDB 和 BDB表。

作为 SQL 标准的扩展，MySQL 也支持整数类型 TINYINT、MEDIUMINT 和 BIGINT。下面的表显示了需要的每个整数类型的存储和范围。



| 类型         | 大小                                     |                        范围（有符号）                        | 范围（无符号）                                               | 用途            |
| :----------- | :--------------------------------------- | :----------------------------------------------------------: | :----------------------------------------------------------- | :-------------- |
| TINYINT      | 1 Bytes                                  |                         (-128，127)                          | (0，255)                                                     | 小整数值        |
| SMALLINT     | 2 Bytes                                  |                      (-32 768，32 767)                       | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    | 3 Bytes                                  |                   (-8 388 608，8 388 607)                    | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 Bytes                                  |               (-2 147 483 648，2 147 483 647)                | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       | 8 Bytes                                  |   (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)    | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        | 4 Bytes                                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       | 8 Bytes                                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 |                        依赖于M和D的值                        | 依赖于M和D的值                                               | 小数值          |

表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'               | YYYY-MM-DD hh:mm:ss | 混合日期和时间值         |
| TIMESTAMP | 4             | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值，时间戳 |

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。

枚举与集合类型（Enumeration and Set Types）

- **ENUM**: 枚举类型，用于存储单一值，可以选择一个预定义的集合。

- **SET**: 集合类型，用于存储多个值，可以选择多个预定义的集合。

  

## 数据表

### 创建数据表

创建 MySQL 数据表需要以下信息：

- 表名
- 表字段名
- 定义每个表字段的数据类型

```
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
);
```

**参数说明：**

- `table_name` 是你要创建的表的名称。

- `column1`, `column2`, ... 是表中的列名。

- `datatype` 是每个列的数据类型。

  实例：

```
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    birthdate DATE,
    is_active BOOLEAN DEFAULT TRUE
);
```

实例解析：

- `id`: 用户 id，整数类型，自增长，作为主键。
- `username`: 用户名，变长字符串，不允许为空。
- `email`: 用户邮箱，变长字符串，不允许为空。
- `birthdate`: 用户的生日，日期类型。
- `is_active`: 用户是否已经激活，布尔类型，默认值为 true。

如果你希望在创建表时指定数据引擎，字符集和排序规则等，可以使用 **CHARACTER SET** 和 **COLLATE** 子句：

```
CREATE TABLE mytable (
    id INT PRIMARY KEY,
    name VARCHAR(50)
) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

```
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

实例解析：

- 如果你不想字段为**空**可以设置字段的属性为 **NOT NULL**，如上实例中的 runoob_title 与 runoob_author 字段， 在操作数据库时如果输入该字段的数据为空，就会报错。
- **AUTO_INCREMENT** 定义列为自增的属性，一般用于主键，数值会自动加 1。
- **PRIMARY KEY** 关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号 **,** 分隔。
- **ENGINE** 设置存储引擎，**CHARSET** 设置编码。

### 删除数据表

```
DROP TABLE table_name ;    -- 直接删除表，不检查是否存在
或
DROP TABLE [IF EXISTS] table_name;

```

```
mysqladmin -u your_username -p drop your_table
```

mysqladmin命令

```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
  die('连接失败: ' . mysqli_error($conn));
}
echo '连接成功<br />';
$sql = "DROP TABLE runoob_tbl";
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
  die('数据表删除失败: ' . mysqli_error($conn));
}
echo "数据表删除成功\n";
mysqli_close($conn);
?>
```

php脚本命令



## 数据操作

### 插入数据

MySQL 表中使用 **INSERT INTO** 语句来插入数据。

你可以通过 **mysql>** 命令提示窗口中向数据表中插入数据，或者通过PHP脚本来插入数据。

以下为向MySQL数据表插入数据通用的 **INSERT INTO** SQL语法：

```
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);

```

- `table_name` 是你要插入数据的表的名称。
- `column1`, `column2`, `column3`, ... 是表中的列名。
- `value1`, `value2`, `value3`, ... 是要插入的具体数值。

如果数据是字符型，必须使用单引号 **'** 或者双引号 **"**，如： 'value1', "value1"。

如果你要插入所有列的数据，可以省略列名：

```
INSERT INTO users
VALUES (NULL,'test', 'test@runoob.com', '1990-01-01', true);
```

这里，**NULL** 是用于自增长列的占位符，表示系统将为 **id** 列生成一个唯一的值。

如果你要插入多行数据，可以在 VALUES 子句中指定多组数值：

```
INSERT INTO users (username, email, birthdate, is_active)
VALUES
    ('test1', 'test1@runoob.com', '1985-07-10', true),
    ('test2', 'test2@runoob.com', '1988-11-25', false),
    ('test3', 'test3@runoob.com', '1993-05-03', true);
```

cmd窗口

```
root@host# mysql -u root -p password;
Enter password:*******
mysql> USE RUNOOB;
DATABASE changed
mysql> INSERT INTO runoob_tbl 
    -> (runoob_title, runoob_author, submission_date)
    -> VALUES
    -> ("学习 PHP", "菜鸟教程", NOW());
Query OK, 1 ROWS affected, 1 warnings (0.01 sec)
mysql> INSERT INTO runoob_tbl
    -> (runoob_title, runoob_author, submission_date)
    -> VALUES
    -> ("学习 MySQL", "菜鸟教程", NOW());
Query OK, 1 ROWS affected, 1 warnings (0.01 sec)
mysql> INSERT INTO runoob_tbl
    -> (runoob_title, runoob_author, submission_date)
    -> VALUES
    -> ("JAVA 教程", "RUNOOB.COM", '2016-05-06');
Query OK, 1 ROWS affected (0.00 sec)
mysql>
```

php脚本

```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
  die('连接失败: ' . mysqli_error($conn));
}
echo '连接成功<br />';
// 设置编码，防止中文乱码
mysqli_query($conn , "set names utf8");
 
$runoob_title = '学习 Python';
$runoob_author = 'RUNOOB.COM';
$submission_date = '2016-03-06';
 
$sql = "INSERT INTO runoob_tbl ".
        "(runoob_title,runoob_author, submission_date) ".
        "VALUES ".
        "('$runoob_title','$runoob_author','$submission_date')";
 
 
 
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
  die('无法插入数据: ' . mysqli_error($conn));
}
echo "数据插入成功\n";
mysqli_close($conn);
?>
```

### 查询数据

MySQL 数据库使用 **SELECT** 语句来查询数据。

```mysql
SELECT column1, column2, ...
FROM table_name
[WHERE condition]
[ORDER BY column_name [ASC | DESC]]
[LIMIT number];

```

- `olumn1`, `column2`, ... 是你想要选择的列的名称，如果使用 `*` 表示选择所有列。
- `table_name` 是你要从中查询数据的表的名称。
- `WHERE condition` 是一个可选的子句，用于指定过滤条件，只返回符合条件的行。
- `ORDER BY column_name [ASC | DESC]` 是一个可选的子句，用于指定结果集的排序顺序，默认是升序（ASC）。
- `LIMIT number` 是一个可选的子句，用于限制返回的行数。

实例

```
-- 选择所有列的所有行
SELECT * FROM users;

-- 选择特定列的所有行
SELECT username, email FROM users;

-- 添加 WHERE 子句，选择满足条件的行
SELECT * FROM users WHERE is_active = TRUE;

-- 添加 ORDER BY 子句，按照某列的升序排序
SELECT * FROM users ORDER BY birthdate;

-- 添加 ORDER BY 子句，按照某列的降序排序
SELECT * FROM users ORDER BY birthdate DESC;

-- 添加 LIMIT 子句，限制返回的行数
SELECT * FROM users LIMIT 10;
```

在 `WHERE` 子句中，你可以使用各种条件运算符（如 `=`, `<`, `>`, `<=`, `>=`, `!=`），逻辑运算符（如 `AND`, `OR`, `NOT`），以及通配符（如 `%`）等。

```
-- 使用 AND 运算符和通配符
SELECT * FROM users WHERE username LIKE 'j%' AND is_active = TRUE;

-- 使用 OR 运算符
SELECT * FROM users WHERE is_active = TRUE OR birthdate < '1990-01-01';

-- 使用 IN 子句
SELECT * FROM users WHERE birthdate IN ('1990-01-01', '1992-03-15', '1993-05-03');
```

[^]: 这里的 `LIKE 'j%'` 使用了通配符 `%`，表示任意数量的字符

通配符 `%` 的作用是匹配零个或多个字符。例如：

- `'j%'`：匹配以 'j' 开头的所有字符串。
- `'%j'`：匹配以 'j' 结尾的所有字符串。
- `'%j%'`：匹配包含 'j' 的所有字符串。

php脚本



```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
// 设置编码，防止中文乱码
mysqli_query($conn , "set names utf8");
 
$sql = 'SELECT runoob_id, runoob_title, 
        runoob_author, submission_date
        FROM runoob_tbl';
 
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('无法读取数据: ' . mysqli_error($conn));
}
echo '<h2>菜鸟教程 mysqli_fetch_array 测试</h2>';
echo '<table border="1"><tr><td>教程 ID</td><td>标题</td><td>作者</td><td>提交日期</td></tr>';
while($row = mysqli_fetch_array($retval, MYSQLI_ASSOC))
{
    echo "<tr><td> {$row['runoob_id']}</td> ".
         "<td>{$row['runoob_title']} </td> ".
         "<td>{$row['runoob_author']} </td> ".
         "<td>{$row['submission_date']} </td> ".
         "</tr>";
}
echo '</table>';
mysqli_close($conn);
?>
```

###  WHERE 子句

我们知道从 MySQL 表中使用 **SELECT** 语句来读取数据。

如需有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句中。

WHERE 子句用于在 MySQL 中过滤查询结果，只返回满足特定条件的行。

```
SELECT column1, column2, ...
FROM table_name
WHERE condition;

```

- 查询语句中你可以使用一个或者多个表，表之间使用逗号**,** 分割，并使用WHERE语句来设定查询条件。
- 你可以在 WHERE 子句中指定任何条件。
- 你可以使用 AND 或者 OR 指定一个或多个条件。
- WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令。
- WHERE 子句类似于程序语言中的 if 条件，根据 MySQL 表中的字段值来读取指定的数据。

下表中实例假定 A 为 10, B 为 20

| 操作符 | 描述                                                         | 实例                 |
| :----- | :----------------------------------------------------------- | :------------------- |
| =      | 等号，检测两个值是否相等，如果相等返回true                   | (A = B) 返回false。  |
| <>, != | 不等于，检测两个值是否相等，如果不相等返回true               | (A != B) 返回 true。 |
| >      | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true | (A > B) 返回false。  |
| <      | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true | (A < B) 返回 true。  |
| >=     | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true | (A >= B) 返回false。 |
| <=     | 小于等于号，检测左边的值是否小于或等于右边的值, 如果左边的值小于或等于右边的值返回true | (A <= B) 返回 true。 |

#### 简单实例

1. 等于条件：

```
SELECT * FROM users WHERE username = 'test';
```

2. 不等于条件：

```
SELECT * FROM users WHERE username != 'runoob';
```

3. 大于条件:

```
SELECT * FROM products WHERE price > 50.00;
```

4. 小于条件:

```
SELECT * FROM orders WHERE order_date < '2023-01-01';
```

5. 大于等于条件:

```
SELECT * FROM employees WHERE salary >= 50000;
```

6. 小于等于条件:

```
SELECT * FROM students WHERE age <= 21;
```

7. 组合条件（AND、OR）:

```
SELECT * FROM products WHERE category = 'Electronics' AND price > 100.00;

SELECT * FROM orders WHERE order_date >= '2023-01-01' OR total_amount > 1000.00;
```

8. 模糊匹配条件（LIKE）:

```
SELECT * FROM customers WHERE first_name LIKE 'J%';
```

9. IN 条件:

```
SELECT * FROM countries WHERE country_code IN ('US', 'CA', 'MX');
```

10. NOT 条件:

```
SELECT * FROM products WHERE NOT category = 'Clothing';
```

11. BETWEEN 条件:

```
SELECT * FROM orders WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31';
```

12. IS NULL 条件

```
SELECT * FROM employees WHERE department IS NULL;
```

13. IS NOT NULL 条件:

```
SELECT * FROM customers WHERE email IS NOT NULL;
```

php脚本

```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
// 设置编码，防止中文乱码
mysqli_query($conn , "set names utf8");
 
// 读取 runoob_author 为 RUNOOB.COM 的数据
$sql = 'SELECT runoob_id, runoob_title, 
        runoob_author, submission_date
        FROM runoob_tbl
        WHERE runoob_author="RUNOOB.COM"';
 
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('无法读取数据: ' . mysqli_error($conn));
}
echo '<h2>菜鸟教程 MySQL WHERE 子句测试<h2>';
echo '<table border="1"><tr><td>教程 ID</td><td>标题</td><td>作者</td><td>提交日期</td></tr>';
while($row = mysqli_fetch_array($retval, MYSQLI_ASSOC))
{
    echo "<tr><td> {$row['runoob_id']}</td> ".
         "<td>{$row['runoob_title']} </td> ".
         "<td>{$row['runoob_author']} </td> ".
         "<td>{$row['submission_date']} </td> ".
         "</tr>";
}
echo '</table>';
// 释放内存
mysqli_free_result($retval);
mysqli_close($conn);
?>
```

### UPDATE 更新

如果我们需要修改或更新 MySQL 中的数据，我们可以使用 **UPDATE** 命令来操作。

以下是 UPDATE 命令修改 MySQL 数据表数据的通用 SQL 语法：

```
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;

```

- `table_name` 是你要更新数据的表的名称。
- `column1`, `column2`, ... 是你要更新的列的名称。
- `value1`, `value2`, ... 是新的值，用于替换旧的值。
- `WHERE condition` 是一个可选的子句，用于指定更新的行。如果省略 `WHERE` 子句，将更新表中的所有行。

- 你可以同时更新一个或多个字段。
- 你可以在 WHERE 子句中指定任何条件。
- 你可以在一个单独表中同时更新数据。

实例

1. 更新单个列的值：

```
UPDATE employees
SET salary = 60000
WHERE employee_id = 101;
```

2. 更新多个列的值：

```
UPDATE orders
SET status = 'Shipped', ship_date = '2023-03-01'
WHERE order_id = 1001;
```

3. 使用表达式更新值：

```
UPDATE products
SET price = price * 1.1
WHERE category = 'Electronics';
```

以上 SQL 语句将每个属于 'Electronics' 类别的产品的价格都增加了 10%。

4. 更新符合条件的所有行：

```
UPDATE students
SET status = 'Graduated';
```

以上 SQL 语句将所有学生的状态更新为 'Graduated'。

5. 更新使用子查询的值：

```
UPDATE customers
SET total_purchases = (
    SELECT SUM(amount)
    FROM orders
    WHERE orders.customer_id = customers.customer_id
)
WHERE customer_type = 'Premium';
```

以上 SQL 语句通过子查询计算每个 'Premium' 类型客户的总购买金额，并将该值更新到 total_purchases 列中。

PHP脚本

```
<?php
$dbhost = 'localhost';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
// 设置编码，防止中文乱码
mysqli_query($conn , "set names utf8");
 
$sql = 'UPDATE runoob_tbl
        SET runoob_title="学习 Python"
        WHERE runoob_id=3';
 
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('无法更新数据: ' . mysqli_error($conn));
}
echo '数据更新成功！';
mysqli_close($conn);
?>
```

