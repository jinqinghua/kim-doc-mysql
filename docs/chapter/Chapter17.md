
<!-- # 第17章 MySQL日志 -->

## 17.1、日志简介

- 二进制日志
- 错误日志
- 通用查询日志
- 慢查询日志

## 17.2、二进制日志

二进制日志也叫作变更日志（update log），主要用于记录数据库的变化情况。通过二进制日志可以查询MySQL数据库中进行了哪些改变。

### 17.2.1、启动和设置二进制日志

默认关闭

```ini
## my.cnf（Linux操作系统下）或者my.ini（Windows操作系统下）
[mysqld]
log-bin [=DIR \ [filename] ]
```

DIR和filename可以不指定，默认为 hostname-bin.number，同时生成 hostname-bin.index 文件

### 17.2.2、查看二进制日志

```sql
mysqlbinlog filename.number
```

### 17.2.3、删除二进制日志

1. 删除所有二进制日志

```sql
RESET MASTER;
```

2. 根据编号来删除二进制日志

```sql
PURGE MASTER LOGS TO 'filename.number'
-- 清除编号小于number的所有二进制文件
```

3. 根据创建时间来删除二进制日志

```sql
PURGE MASTER LOGS TO 'yyyy-mm-dd hh:MM:ss'
-- 删除指定时间之前的
```

### 17.2.4、使用二进制日志还原数据库

```sql
mysqlbinlog filename.number | mysql -u root –p

-- number编号小的先还原
```

### 17.2.5、暂时停止二进制日志功能

```sql
SET SQL_LOG_BIN=0
```

## 17.3、错误日志

错误日志是MySQL数据库中最常用的一种日志。错误日志主要用来记录MySQL服务的开启、关闭和错误信息。

### 17.3.1、启动和设置错误日志

默认开启的，而且，错误日志无法被禁止。

默认情况下，错误日志存储在MySQL数据库的数据文件夹下。错误日志文件通常的名称为hostname.err。其中，hostname表示MySQL服务器的主机名。错误日志的存储位置可以通过log-error选项来设置。将log-error选项加入到my.ini或者my.cnf文件的[mysqld]组中，形式如下：

```ini
## my.cnf（Linux操作系统下）或者my.ini（Windows操作系统下）
[mysqld]
log-error=DIR / [filename]
```

### 17.3.2、查看错误日志

文本编辑/查看器

### 17.3.3、删除错误日志

MySQL 数据库中，可以使用mysqladmin命令来开启新的错误日志。mysqladmin命令的语法如下：

```sql
mysqladmin -u root -p flush-logs
```

执行该命令后，数据库系统会自动创建一个新的错误日志。旧的错误日志仍然保留着，只是已经更名为filename.err-old。

## 17.4、通用查询日志

通用查询日志用来记录用户的所有操作，包括启动和关闭MySQL服务、更新语句、查询语句等。

### 17.4.1、启动和设置通用查询日志

默认情况下，通用查询日志功能是关闭的

通过my.cnf或者my.ini文件的log选项可以开启通用查询日志。将log选项加入到my.cnf或者my.ini文件的[mysqld]组中，形式如下：

```ini
## my.cnf（Linux操作系统下）或者my.ini（Windows操作系统下）
[mysqld]
log [=DIR \ [filename] ]
```

### 17.4.2、查看错误日志

文本编辑/查看器

### 17.4.3、删除通用查询日志

可以使用mysqladmin命令来开启新的通用查询日志。新的通用查询日志会直接覆盖旧的查询日志，不需要再手动删除了。mysqladmin命令的语法如下：

```sql
mysqladmin -u root -p flush-logs
```

## 17.5、慢查询日志

慢查询日志用来记录执行时间超过指定时间的查询语句。通过慢查询日志，可以查找出哪些查询语句的执行效率很低，以便进行优化。

### 17.5.1、启动和设置慢查询日志

默认情况下，慢查询日志功能是关闭的。

通过my.cnf或者my.ini文件的log-slow-queries选项可以开启慢查询日志。通过long_query_time选项来设置时间值，时间以秒为单位。如果查询时间超过了这个时间值，这个查询语句将被记录到慢查询日志。将log-slow-queries选项和long_query_time选项加入到my.cnf或者my.ini文件的[mysqld]组中，形式如下：

```ini
## my.cnf（Linux操作系统下）或者my.ini（Windows操作系统下）
[mysqld]
log-slow-queries [=DIR \ [filename] ]
long_query_time=n
```

### 17.5.2、查看慢查询日志

文本编辑/查看器

### 17.5.3、删除慢查询日志

慢查询日志的删除方法与通用查询日志的删除方法是一样的。可以使用mysqladmin命令来删除。也可以使用手工方式来删除。mysqladmin命令的语法如下：

```sql
mysqladmin -u root -p flush-logs
```

执行该命令后，命令行会提示输入密码。输入正确密码后，将执行删除操作。新的慢查询日志会直接覆盖旧的查询日志，不需要再手动删除了。数据库管理员也可以手工删除慢查询日志。删除之后需要重新启动MySQL服务。重启之后就会生成新的慢查询日志。如果希望备份旧的慢查询日志文件，可以将旧的日志文件改名。然后重启MySQL服务

## 17.6、小结

| 日志类型    | 配置 my.cnf或my.ini                               | 默认 启动 | 查看                      | 删除                                                     |
| --------------- | ------------------------------------------------------- | --------------- | ----------------------------- | ------------------------------------------------------------ |
| 二进制日志   | [mysqld]  log-bin [=DIR \ [filename] ]                  | 否              | mysqlbinlog   filename.number | RESET MASTER;  <br />PURGE MASTER LOGS TO   'filename.number'  <br />PURGE MASTER LOGS TO   'yyyy-mm-dd  hh:MM:ss' |
| 错误日志     | [mysqld]  log-error[=DIR \ [filename] ]                 | 是              | 文本查看/编辑器               | mysqladmin -uroot -p flush-logs                              |
| 通用查询日志 | [mysqld]  log [=DIR \ [filename] ]                      | 否              | 文本查看/编辑器               | mysqladmin -uroot -p flush-logs                              |
| 慢查询日志   | log-slow-queries[=DIR \ [filename] ]  long_query_time=n | 否              | 文本查看/编辑器               | mysqladmin -uroot -p flush-logs                              |
