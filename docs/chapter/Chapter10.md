<!-- # 第10章 查询数据 -->

## 10.1、基本查询语句

```sql
SELECT 属性列表 
     FROM 表名和视图列表
     [WHERE 条件表达式1]
     [GROUP BY 属性名1 [HAVING条件表达式2]]
     [ORDER BY 属性名2[ASC|DESC]]
```

## 10.2、单表查询

- 列出所有字段 *
- 指定字段 f1, f2, f3
- 指定记录
```sql
     WHERE 条件表达式
     =,<,>,!及其组合
     [NOT] BETWEEN AND
     [NOT] IN
     [NOT] LIKE
     %
     _
     IS [NOT] NULL
     AND,OR

SELECT DISTINCT 属性名
ORDER BY 属性名 [ASC|DESC]
GROUP BY, GROUP_CONTACT() -- 函数非常好用
     SELECT sex, GROUP_CONTACT(name) FROM employee GROUP BY sex;
GROUP BY -- 与 WITH ROLLUP 一起使用，多一行，加统计
     SELECT sex COUNT(sex) FROM employee GROUP BY sex WITH ROLLUP;
LIMIT [初始位置,] 记录数
```

## 10.3、使用集合函数查询

```sql
COUNT()
AVG()
MAX()
MIN()
SUM()
```

## 10.4、连接查询

### 10.4.1、内连接查询

```sql
select a.*, b.* from a, b where a.xid=b.xid
```

### 10.4.2、外连接查询

```sql
SELECT 属性名列表 FROM 表名1 LEFT|RIGHT JOIN 表名2
	ON 表名1.属性1＝表名2.属性2；
    LEFT JOIN     -- 左表全记录，右表符合条件
    RIGHT JOIN    -- 右表全记录，左表符合条件
```

## 10.5、子查询

IN

EXISTS 表示存在，内层查询语句不返回查询的记录，而是返回一个真假值 （true|false）

ANY任意一个值

```sql
 SELECT * FROM computer_stu WHERE scrore >=ANY(SELECT score FROM scholarship)
```

ALL 满足所有条件

## 10.6、合并查询结果

```sql
SELECT 语句1
	UNION | UNION ALL
SELECT 语句2
...
```

UNION 所有的查询结果合并到一起，去掉重复项

UNION ALL 简单合并

## 10.7、为表和字段取别名

```sql
表名 表的别名
属性名 [AS] 属性的别名
```

## 10.8、使用正则表达式查询

```sql
属性名 REGEXP '匹配方式'

^             字符串开始
$             字符串结束
.             任意一个字符，包括回车和换行
[字符集合]     匹配字符集合中的任一字符
S1|S2|S3      三者之任一
\*            任意多个
\+            1+个
字符串{N}      字符串出现N次
字符串{M,N}    字符串出现至少M次，至多N次

SELECT * FROM info WHERE name REGEXP 'ab{1,3}';
```
