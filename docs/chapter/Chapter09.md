<!-- # 第 9 章 触发器 -->

触发器（TRIGGER）是由事件来触发某个操作。这些事件包括INSERT语句、UPDATE语句和DELETE语句。当数据库系统执行这些事件时，就会激活触发器执行相应的操作。MySQL从5.0.2版本开始支持触发器

## 9.1、创建触发器

#### 9.1.1、创建只有一个执行语句的触发器

```sql
CREATE TRIGGER 触发器名 BEFORE | AFTER 触发事件
ON 表名 FOR EACH ROW 执行语句
```

#### 9.1.2、创建有多个执行语句的触发器

```sql
DELIMITER &&

CREATE TRIGGER 触发器名 BEFORE | AFTER 触发事件
ON 表名 FOR EACH ROW 

BEGIN
执行语句列表
END

&&

DELEMITER ；

-- DELIMITER，一般SQL "；"结束，在创建多个语句执行的触发器时，要用到"；"，所以用DELIMETER来切换一下。

CREATE TRIGGER dept_tig1 BEFORE INSERT ON department
FOR EACH ROW
INSERT INTO trigger_time VALUES(NOWS());
```

## 9.2、查看触发器

```sql
SHOW TRIGGERS ;
SELECT * FROM information_schema. triggers ;
```

## 9.3、触发器的使用

MySQL中，触发器执行的顺序是BEFORE触发器、表操作（INSERT、UPDATE 和DELETE）、AFTER触发器

触发器中不能包含START TRANSACTION, COMMIT, ROLLBACK,CALL等。

## 9.4、删除触发器

```sql
DROP TRIGGER 触发器名 ;
```