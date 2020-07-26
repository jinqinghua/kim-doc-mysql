<!-- # 第 2 章 Windows平台下安装与配置MySQL -->

## 2.1、msi安装包

### 2.1.1、安装

特别要注意的是，安装前要删除原来的my.ini和原来的data目录，改名也行，不然在最后一步会"apply security settings"报个1045错误，原因1，防火墙，原因2，数据文件未清除。

一路next，选custom安装

可以指定data的位置，不要指定到系统盘

顺便配置，选择"detailed configuration"

服务器类型和用途开发还是生产环境

"best support for multilingualism"  支持大部分语系，默认字符集是UTF-8，用这个吧

"add firewall exception for this port" 最好选上，尤其在开发机

"enabled strict mode"  生产机推荐，开发机可以不用，选的话，容易出现刚开始要求注意的问题

"include bin directory in windows path"  强烈建议选上，不然要手动配置path路径

"create anonymous account" 就不要了

没有意外的话，成功搞定

安装后root登录不了的解决办法

```shell
mysql -h localhost -u root -p
net stop mysql
mysqld --skip-grant-tables
```

\#注意，`net start mysql --skip-grant-tables`，能启动，但是好象达不到效果

窗口可能死掉，不管，另开一个窗口

```shell
mysql -u root
```

发现直接进去了

```sql
use mysql
update user set password=password("新密码") where user='root' and host='localhost';
flush previliges;
```

OK了，注意几点：

1. net start mysql --skip-grant-tables，能启动，但是好象达不到效果
2. mysql是内置的数据库
3. user表是mysql库里存储用户名密码和权限的表
4. 密码要用password()函数加密一下
5. host='localhost'这个条件可以不要的，那么root所有的密码都变了，不建议这样做，后面会简单讲一下mysql的用户
6. 此时set方法mysqlamdin -u root -p password "新密码"的修改密码方法是行不通的，只有直接修改数据库

### 2.1.2、卸载

1. 可以在控制面板里卸载
2. 最好通过原来安装包，双击，选"remove"卸载，较彻底

## 2.2、zip文件（未验证）

### 2.2.1、安装

1. 下载mysql
2. 解压到c:/mysql
3. 复制my-large.ini 到c:/windows/my.ini
4. 修改my.ini文件

```ini
basedir="c:/mysql" 安装目录
datadir="c:/mysql/data" 数据目录
[WindowsMySQLServer]
Server="c:/mysql/bin/mysqld.exe"
```
5. 安装服务

```sql
c:/mysql/bin/mysqld.exe --install
```
6. 启动/关闭服务

```sql
net start/stop mysql
```

### 2.2.2、卸载

```sql
c:/mysql/bin/mysqld.exe --remove
```

## 2.3、命令常用参数及使用方法

```
List of all MySQL commands:

Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.（清除输入，不执行）
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
ego       (\G) Send command to mysql server, display result vertically.（垂直显示）
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
notee     (\t) Don't write into outfile.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.

For server side help, type 'help contents'
```

### 2.3.1、mysql

```mysql
-h host
-u user
-p password  -- 注意，一般不输密码，如果输，和-p之间不能有空格)
-P port      -- 一般是3306不常用
databasename -- 数据库名，相当于执行了use database
-e "sql"     -- 执行语句

mysql -h localhost -u root -ppassword mysql -e "select user,host from user"
```

### 2.3.2、mysqladmin

修改密码

```mysql
mysqladmin -u root -p password "新密码"
```

注意：
1. password相当于函数，必须要的
2. 新密码要用双引号扩起来
