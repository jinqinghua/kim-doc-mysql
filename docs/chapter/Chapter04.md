<!-- # 第 4 章 MySQL数据类型 -->

## 4.1、整数类型

```sql
tinyint(4)
smallint(6)
mediumint(9)
int(11)
bigint(20)
```
注意：后面的是默认显示宽度，以int为例，占用的存储字节数是4个，即4*8=32位，2的32次方，无符号的最大能达到4亿多。

tinyint(4)相当于bool型

## 4.2、浮点数

```sql
float
double
decimal(m,d)
decimal(6,2)  -- 定义的数字形如1234.56，指总长6位，小数点后精确到2位
```

## 4.3、日期和时间

```sql
year  -- 年
date  -- 日期
time  -- 时间
datetime  -- 日期时间
timestamp -- 时间（时区），范围小，支持时区
datetime  -- 最通用，year,date,time 可以节省一些空间。
```

## 4.4、字符串

```sql
char(m)    -- 定长
varchar(m) -- 不定长
enum,set   -- 和其它库不兼容，可暂不用
tinytext
text
mediumtext
longtext
```

## 4.5、二进制

```sql
binary(m)
varbinary(m)
bit(m)
tinyblob
blob
mediumblob
longblob
```
