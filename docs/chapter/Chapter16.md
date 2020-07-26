<!-- # 第16章 数据备份与还原 -->

## 16.1、数据备份

### 16.1.1、使用 mysqldump 命令备份

```sql
mysqldump [OPTIONS] database [tables]
mysqldump [OPTIONS] --databases [OPTIONS] DB1 [DB2 DB3...]
mysqldump [OPTIONS] --all-databases [OPTIONS]

mysqldump –u root –p test student >c:/student.sql
mysqldump –u root –p test mysql > c:/multidb.sql
mysqldump –u root –p –all-databases > c:/all.sql
```

### 16.1.2、直接复制整个数据库目录

MyISAM 存储引擎的的表适用

大版本号相同数据库数据库文件格式相同

### 16.1.3、使用mysqlhotcopy工具快速备份

Linux下备份，perl脚本。

## 16.2、数据还原

### 16.2.1、使用mysql命令还原

```sql
mysql –u root –p [dbname] < backup.sql
mysql –u root –p < all.sql
```

### 16.2.2、直接复制到数据库目录

## 16.3、数据库迁移

### 16.3.1、相同版本的MySQL数据库之间的迁移

```sql
mysqldump –h host1 –u root –password=password1 –all-databases | mysql –h host2 –u root –password=password2

mysqldump –h host1 –u root –ppassword databasename | mysql –h host2 –u root –ppassword databasename
```

### 16.3.2、不同版本的MySQL数据库之间的迁移

```sql
mysqldump
```

### 16.3.3、不同数据库之间的迁移

1. 工具，如MS SQL Server的数据库迁移工具

2. dump出 sql 语句，然后手工修改create语句

## 16.4、表的导出和导入

### 16.4.1、用SELECT…INTO OUTFILE导出文本文件

```sql
SELECT [列名] FROM table [WHERE语句]
INTO OUTFILE '目标文件' [OPTION]
```

能根据条件导出数据

### 16.4.2、用mysqldump命令导出文本文件

```sql
mysqldump –u root –pPassword –T 目标目录或文件 dbname table [option];

--fields-terminated-by=...，
--fields-enclosed-by=...，
--fields-optionally-enclosed-by=...，
--fields-escaped-by=...，
--fields-terminated-by=...
```

导出的是txt + sql文件

### 16.4.3、用mysql命令导出文本文件

```sql
mysql –u root –pPassword –e "sql"dbname > c:/sql.txt
mysql –u root –pPassword --xml | -X -e "sql"dbname > c:/sql.txt
mysql –u root –pPassword --html | -H -e "sql"dbname > c:/sql.txt
```

### 16.4.4、用LOAD DATA INFILE方式导入文本文件

```sql
LOAD DATA[LOCAL] INFILE file INTO TABLE table [OPTION]
LOAD DATA INFILE C:/student.txt INTO TABLE student [OPTION]
```

### 16.4.5、用mysqlimport命令导入文本文件

```sql
mysqlimport –u root –pPassword [--LOCAL] dbname file [OPTION]
```
