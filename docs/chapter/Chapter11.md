<!-- # 第11章 插入、更新与删除数据 -->

## 11.1、插入数据

### 11.1.1、为表的所有字段插入数据

1、INSERT语句中不指定具体的字段名

```sql
insert into 表名 values (值1,值2…值n)
```

2、INSERT语句中列出所有字段

```sql
insert into 表名 (属性1,属性2…属性n) values (值1,值2…值n)
```

### 11.1.2、为表的指定字段插入数据

```sql
insert into 表名 (属性1,属性2,属性3) values (值1,值2,值3)
```

### 11.1.3、同时插入多条数据

```sql
insert into 表名 (属性1,属性2…属性n) values
	(值1,值2…值n),
	(值1,值2…值n),
	...
	(值1,值2…值n);
```

MySQL特有的

### 11.1.4、将查询结果插入到表中

```sql
insert into 表名 (属性1,属性2…属性n) 
   select 属性列表 from 表名2 where 条件表达式

use mysql;
create table user1 (user char(16) not null);
insert into user1 (user) select user from user;
```

## 11.2、更新数据

```sql
update 表名
	set 属性1=值1,属性2＝值2，…
	where 条件表达式;
```

## 11.3、删除数据

```sql
delete from 表名 [条件表达式];
```