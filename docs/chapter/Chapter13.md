<!-- # 第13章 MySQL函数 -->

## 13.1、数学函数

​     随机数可能会用到，其他基本无视。

## 13.2、字符串函数

​     重点CONCAT(S1,S2….)

## 13.3、日期和时间函数

​     重点

## 13.4、条件判断函数

- IF(expr,v1,v2)
- IFNULL(v1,v2)
- CASE
```sql
CASE WHEN expr1 THEN v1 [WHEN expr2 THEN v2…] [ELSE vn] END
CASE expr WHEN e1 THEN v1 [WHEN e2 THEN v2…] [ELSE vn] END
```

## 13.5、系统信息函数

VERSION()函数返回数据库的版本号；

CONNECTION_ID()函数返回服务器的连接数，也就是到现在为止MySQL服务的连接次数；

DATABASE()和SCHEMA()返回当前数据库名。

USER()、SYSTEM_USER()、SESSION_USER()、CURRENT_USER()和CURRENT_USER这几个函数可以返回当前用户的名称

CHARSET(str) 函数返回字符串str的字符集，一般情况这个字符集就是系统的默认字符集；

COLLATION(str) 函数返回字符串str的字符排列方式。

LAST_INSERT_ID()函数返回最后生成的AUTO_INCREMENT值。

## 13.6、加密函数

PASSWORD(str)函数可以对字符串str进行加密。一般情况下，PASSWORD(str)函数主要是用来给用户的密码加密的。

MD5(str)函数可以对字符串str进行加密。MD5(str)函数主要对普通的数据进行加密。

ENCODE(str,pswd_str)函数可以使用字符串pswd_str来加密字符串str。加密的结果是一个二进制数，必须使用BLOB类型的字段来保存它。

DECODE(crypt_str,pswd_str)函数可以使用字符串pswd_str来为crypt_str解密

## 13.7、格式化函数

FORMAT(x,n)函数可以将数字x进行格式化，将x保留到小数点后n位。这个过程需要进行四舍五入。

ASCII(s)返回字符串s的第一个字符的ASCII码；

BIN(x)返回x的二进制编码；

HEX(x)返回x的十六进制编码；

OCT(x)返回x的八进制编码；

CONV(x,f1,f2)将x从f1进制数变成f2进制数。

INET_ATON(IP)函数可以将IP地址转换为数字表示；INET_NTOA(n)函数可以将数字n转换成IP的形式。其中，INET_ATON(IP)函数中IP值需要加上引号。这两个函数互为反函数。

GET_LOCT(name,time)函数定义一个名称为nam、持续时间长度为time秒的锁。如果锁定成功，返回1；如果尝试超时，返回0；如果遇到错误，返回NULL。

RELEASE_LOCK(name)函数解除名称为name的锁。如果解锁成功，返回1；如果尝试超时，返回0；如果解锁失败，返回NULL；

IS_FREE_LOCK(name)函数判断是否使用名为name的锁。如果使用，返回0；否则，返回1。

BENCHMARK(count,expr)函数将表达式expr重复执行count次，然后返回执行时间。该函数可以用来判断MySQL处理表达式的速度。

CONVERT(s USING cs)函数将字符串s的字符集变成cs

CAST(x AS type)和CONVERT(x,type)这两个函数将x变成type类型。这两个函数只对BINARY、CHAR、DATE、DATETIME、TIME、SIGNED INTEGER、UNSIGNED INTEGER这些类型起作用。但两种方法只是改变了输出值的数据类型，并没有改变表中字段的类型