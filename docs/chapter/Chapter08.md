<!-- # 第 8 章 视图 -->

## 8.1、视图简介

视图由数据库中的一个表，视图或多个表，视图导出的虚拟表。其作用是方便用户对数据的操作。


## 8.2、创建视图

必须要有CREATE VIEW 和 SELECT 权限

```sql
SELECT select_priv, create_view_priv from mysql.user WHERE user='root';

CREATE  [ ALGORITHM = { UNDEFINED | MERGE | TEMPTABLE } ]
VIEW 视图名 [ ( 属性清单 ) ]
AS SELECT 语句
[ WITH [ CASCADED | LOCAL ] CHECK OPTION ] ;
```

ALGORITHM参数表示视图选择的算法

UNDEFINED 未指定，自动选择

MERGE 表示将使用视图的语句和视图定义合并起来，使得视图定义的某一部分取代语句的对应部分

TEMPTABLE 表示将视图的结果存入临时表，然后使用临时表执行语句

LOCAL参数表示更新视图时要满足该视图本身定义的条件即可；

CASCADED参数表示更新视图时要满足所有相关视图和表的条件，默认值。

使用CREATE VIEW语句创建视图时，最好加上WITH CHECK OPTION参数和CASCADED参数。这样，从视图上派生出来的新视图后，更新新视图需要考虑其父视图的约束条件。这种方式比较严格，可以保证数据的安全性。

```sql
create view department_view1 as 
  select * from department;

create view department_view2 (name, function, localtion) as 
  select d_name, function, address from department;


create algorithm=merge view worker_view1(name,department, sex, age, address) as 
 select name, department.d_name,sex, 2009-birthday, address from worker, department where worker.d_id=department.d_id
 with local check option;
```

## 8.3、查看视图

必须要有SHOW VIEW的权限

```sql
DESCRIBE｜DESC 视图名 ;
SHOW TABLE STATUS LIKE '视图名' ;
SHOW CREATE VIEW 视图名;
SELECT \* FROM information_schema.views ;
```

## 8.4、修改视图

```sql
CREATE OR REPLACE | ALTER [ ALGORITHM = { UNDEFINED | MERGE | TEMPTABLE } ]
VIEW 视图名 [ ( 属性清单 ) ]
AS SELECT语句
[ WITH [ CASCADED | LOCAL ] CHECK OPTION ] ;
```

语法和CREATE VIEW基本一样

## 8.5、更新视图

更新视图是指通过视图来插入（INSERT）、更新（UPDATE）和删除（DELETE）表中的数据。因为是视图是一个虚拟表，其中没有数据。通过视图更新时，都是转换到基本表来更新。更新视图时，只能更新权限范围内的数据。超出了范围，就不能更新。

原则：尽量不要更新视图

语法和UPDATE语法一样

哪些视图更新不了：

1. 视图中包含SUM(), COUNT()等聚焦函数的

2. 视图中包含UNION、UNION ALL、DISTINCT、GROUP BY、HAVING等关键字

3. 常量视图 `CREATE VIEW view_now AS SELECT NOW()`

4. 视图中包含子查询

5. 由不可更新的视图导出的视图

6. 创建视图时ALGORITHM为TEMPTABLE类型

7. 视图对应的表上存在没有默认值的列，而且该列没有包含在视图里

8. `WITH [CASCADED|LOCAL] CHECK OPTION` 也将决定视图是否可以更新

​     LOCAL 参数表示更新视图时要满足该视图本身定义的条件即可；

​     CASCADED 参数表示更新视图时要满足所有相关视图和表的条件，默认值。

## 8.6 、删除视图

删除视图时，只能删除视图的定义，不会删除数据

用户必须拥有DROP权限

```sql
DROP VIEW [ IF EXISTS] 视图名列表 [ RESTRICT | CASCADE]
```
