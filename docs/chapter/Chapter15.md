<!-- # 第15章 MySQL用户管理 -->

## 15.2、账户管理

## 15.2.1、登录和退出MySQL服务器

```sql
mysql –h hostname|hostIP –P port –u username –p[password] databaseName –e "SQL语句"

-h       -- 主机名或ip
-P       -- port[3306]
-u       -- username
-p       -- -p[password]     注意，之间没有空格
-e       -- 执行SQL语句       SQL用双引号括起
```

可以用此语句配合操作系统定时任务，达到自动处理表数据的功能，如定时将某表中过期的数据删除。

## 15.2.2、新建立普通用户

1、用CREATE USER语句新建

```sql
CREATE USER user [IDENTIFIED BY [PASSWORD]'password']
	[,user [IDENTIFIED BY [PASSWORD]'password']]...
```

2、用INSERT语句来新建普通用户

```sql
INSERT INTO mysql.user(host,user,password,ssl_cipher,x509_issuer,x509_subject) 
VALUES ('localhost','test2',PASSWORD('test2'),'','','');
FLUSH PRIVELEGES;
```

3、用GRANT语句来新建普通用户

```sql
GRANT priv_type ON database.table TO user [IDENTIFIED BY [PASSWORD]'password
[,user [IDENTIFIED BY [PASSWORD]'password']]...
```


## 15.2.3、删除普通用户

1、用DROP USER语句来删除普通用户

```sql
DROP USER user [,user]…;

drop user 'test'@'localhost';
```

2、用DELETE语句来删除普通用户

```sql
DELETE FROM mysql.user WHERE user='username' and host='hostname';

FLUSH PRIVILEGES;
```

## 15.2.4、root用户修改自己的密码

1、使用mysqladmin命令来修改root用户的密码

```sql
mysqladmin –u username –p password "new_password";
```

2、修改user表

```sql
UPDATE mysql.user SET password=PASSWORD("new_password") WHERE user='root' and host='';

FLUSH PRIVILEGES;
```

3、使用SET语句来修改root用户的密码

```sql
SET PASSWORD=PASSWORD("new_password");
```

## 15.2.5、root用户修改普通用户密码

1、使用mysqladmin命令

不适用，mysqladmin只能修改root 用户密码

2、修改user表

```sql
UPDATE mysql.user SET password=PASSWORD("new_password") WHERE user='' and host='';

FLUSH PRIVILEGES;
```

3、使用SET语句来修改普通用户的密码

```sql
SET PASSWORD FOR 'user'@'localhost'=PASSWORD("new_password");
```

4、用GRANT语句来修改普通用户的密码

```sql
GRANT priv_type ON database.table TO user [IDENTIFIED BY [PASSWORD]'password
[,user [IDENTIFIED BY [PASSWORD]'password']]...
```

## 15.2.6、普通用户修改密码

```sql
SET PASSWORD=PASSWORD("new_password");
```

## 15.2.7、root用户密码丢失的解决办法

1、使用—skip-grant-tables选项来启动MySQL服务

```shell
mysqld –skip-grant-tables
#/etc/init.d/mysql start –mysqld –skip-grant-tables
```

2、登录root，设置新密码

```sql
mysql –u root mysql
update mysql.user set password=password("new_password") where user='root' and host='localhost';
```

3、加载权限表

```sql
fush privileges;
```

## 15.3、权限管理

## 15.3.1、MySQL的各种权限

```sql
create, select, update, delete
all [privileges] -- 指所有权限
```

## 15.3.2、授权

```sql
GRANT priv_type [(column_list)] [, priv_type [(column_list)]] ...
  ON [object_type] {tbl_name | * | *.* | db_name.*}
  TO user [IDENTIFIED BY [PASSWORD] 'password']
     [, user [IDENTIFIED BY [PASSWORD] 'password']] ...
  [REQUIRE
     NONE |
     [{SSL| X509}]
     [CIPHER 'cipher' [AND]]
     [ISSUER 'issuer' [AND]]
     [SUBJECT 'subject']]
  [WITH with_option [with_option] ...]

object_type =
  TABLE
 | FUNCTION
 | PROCEDURE

with_option =
  GRANT OPTION
 | MAX_QUERIES_PER_HOUR count
 | MAX_UPDATES_PER_HOUR count
 | MAX_CONNECTIONS_PER_HOUR count
 | MAX_USER_CONNECTIONS count

grant select, insert, update, delete on testdb.* to common_user@'%'
database.table       *.*所有库的所有表
user          'user'@'host'
```

## 15.3.3、收回权限

```sql
REVOKE priv_type [(column_list)] [, priv_type [(column_list)]] ...
  ON [object_type] {tbl_name | * | *.* | db_name.*}
  FROM user [, user] ...

REVOKE ALL PRIVILEGES, GRANT OPTION FROM user [, user] ...
```
