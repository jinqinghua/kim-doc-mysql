<!-- # 第14章 存储过程和函数 -->

避免编写重复的语句

安全性可控

执行效率高

## 14.1、创建存储过程和函数

### 14.1.1、创建存储过程

```sql
CREATE PROCEDURE sp_name ([proc_parameter[,...]])
[characteristic ...] routine_body
-- procedure 			发音 [prə'si:dʒə]
-- proc_parameter       IN|OUT|INOUT param_name type
-- characteristic       n. 特征；特性；特色

     LANGUAGE SQL          默认，routine_boyd由SQL组成
     [NOT] DETERMINISTIC     指明存储过程的执行结果是否是确定的，默认不确定
     CONSTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA指定程序使用SQL语句的限制

    CONSTAINS SQL      -- 子程序包含SQL，但不包含读写数据的语句，默认
    NO SQL             -- 子程序中不包含SQL语句
    READS SQL DATA     -- 子程序中包含读数据的语句
    MODIFIES SQL DATA  -- 子程序中包含了写数据的语句

    SQL SECURITY {DEFINER|INVOKER}，指明谁有权限执行。
		DEFINER，只有定义者自己才能够执行，默认
		INVOKER  表示调用者可以执行
    COMMENT 'string' 注释信息

CREATE PROCEDURE num_from_employee (IN emp_id, INT, OUT count_num INT)
	READS SQL DATA
	BEGIN
		SELECT COUNT() INTO count_num
		FROM employee
		WHERE d_id=emp_id;
	END
```

### 14.1.2、创建存储函数

```sql
CREATE FUNCTION sp_name ([func_parameter[,...]])
RETURNS type
[characteristic ...] routine_body

CREATE FUNCTION name_from_employee(emp_id INT)
	RETURNS VARCHAR(20)
	BEGIN
		RETURN (SELECT name FROM employee WHERE num=emp_id);
	END
```

### 14.1.3、变量的使用

#### 1. 定义变量

```sql
DECLARE var_name[,…] type [DEFAULT value]

DECLARE my_sql INT DEFAULT 10;
```

#### 2. 为变量赋值

```sql
SET var_name=expr[,var_name=expr]…

SELECT col_name[,…] INTO var_name[,…] FROM table_name WHERE condition
```

### 14.1.4、定义条件和处理程序

#### 1. 定义条件

```sql
DECLARE condition_name CONDITION FOR condition_value
condition value:
	SQLSTATE [VALUE] sqlstate_value | mysql_error_code

```

对于ERROR 1146(42S02)

sqlstate_value: 42S02

mysql_error_code:1146

// 方法一

```sql
DECLARE can_not_find CONDITION FOR SQLSTATE '42S02'
```

// 方法二

```sql
DECLARE can_not_find CONDITION FOR 1146
```

#### 2. 定义处理程序

```sql
DECLARE hander_type HANDLER FOR condition_value[,…] sp_statement
```

```sql
handler_type:
	CONTINUE|EXIT|UNDO

condition_value:

SQLSTATE[VALUE] sqlstate_value | condition_name | SQLWARNING|NOTFOUND|SQLEXCEPTION|mysql_error_code
```

UNDO目前MySQL不支持

1、捕获sqlstate_value

```sql
DECLARE CONTINUE HANDLER FOR SQLSTATE '42S02' SET @info='CAN NOT FIND';
```

2、捕获 mysql_error_code

```sql
DECLARE CONTINUE HANDLER FOR 1146 SET @info='CAN NOT FIND';
```

3、先定义条件，然后调用

```sql
DECLARE can_not_find CONDITION FOR 1146;

DECLARE CONTINUE HANDLER FOR can_not_find SET @info='CAN NOT FIND';
```

4、使用 SQLWARNING

```sql
DECLARE EXITHANDLER FOR SQLWARNING SET @info='CAN NOT FIND';
```

5、使用 NOT FOUND

```sql
DECLARE EXIT HANDLER FOR NOT FOUND SET @info='CAN NOT FIND';
```

6、使用 SQLEXCEPTION

```sql
DECLARE EXIT HANDLER FOR SQLEXCEPTION SET @info='CAN NOT FIND';
```

### 14.1.5、光标的使用

存储过程中对多条记录处理，使用光标

#### 1. 声明光标

```sql
DECLARE cousor_name COURSOR FOR select statement;

DECLARE cur_employee CURSOR FOR SELECT name, age FROM employee;

```

#### 2. 打开光标

```sql
OPEN cursor_name;
OPEN cur_employee;
```

#### 3. 使用光标

```sql
FETCH cur_employee INTO var_name[,var_name…];

FETCH cur_employee INTO emp_name, emp_age;
```

#### 4. 关闭光标

```sql
CLOSE cursor_name

CLOSE cur_employee
```

### 14.1.6、流程控制的使用

#### 1. IF语句

```sql
IF search_condition THEN statement_list
	[ELSEIF search_condition THEN statement_list]…
	[ELSE statement_list]
END IF

IF age>20 THEN SET @count1=@count1+1;
	ELSEIF age=20 THEN @count2=@count2+1;
	ELSE @count3=@count3+1;
END
```

#### 2. CASE语句
```sql
CASE case_value
	WHEN when_value THEN statement_list
	[WHEN when_value THEN statement_list]…
	[ELSE statement_list]
END CASE

CASE
	WHEN search_condition THEN statement_list
	[WHEN search_condition THEN statement_list]…
	[ELSE statement_list]
END CASE

CASE age
	WHEN 20 THEN SET @count1=@count1+1;
	ELSE SET @count2=@count2+1;
END CASE;

CASE
	WHERE age=20 THEN SET @count1=@count1+1;
	ELSE SET @count2=@count2+1;
END CASE;
```

#### 3. LOOP语句

```sql
[begin_label:]LOOP
	statement_list
END LOOP[end_label]

add_num:LOOP
	qSET @count=@count+1;
END LOOP add_num;
```

#### 4. LEAVE语句

跳出循环控制

```sql
LEAVE label

add_num:LOOP
	SET @count=@count+1;
	LEAVE add_num;
END LOOP add_num;
```

#### 5. ITERATE语句

跳出本次循环，执行下一次循环

```sql
ITERATE label

add_num:LOOP
	SET @count=@count+1;
	IF @count=100 THEN LEAVE add_num;
	ELSEIF MOD(@count,3)=0 THEN ITERATE add_num;
	SELECT * FROM employee;
END LOOP add_num;
```

#### 6. REPEAT语句

有条件循环，满足条件退出循环

```sql
[begin_label:]REPEAT
	statement_list
	UNTIL search_condition
END REPEAT[end_label]

REPEAT
	SET @count=@count+1;
	UNTIL @count=100;
END REPEAT;
```

#### 7. WHILE语句

```sql
[begin_label:]WHILE search_condition DO
	statement_list
END REPEAT[end_label]

WHILE @count<100 DO
	SET @count=@count+1;
END WHILE;
```

## 14.2、调用存储过程和函数

存储过程是通过CALL语句来调用的。而存储函数的使用方法与MySQL内部函数的使用方法是一样的。执行存储过程和存储函数需要拥有EXECUTE权限。EXECUTE权限的信息存储在information_schema数据库下面的USER_PRIVILEGES表中

### 14.2.1、调用存储过程

```sql
CALL sp_name([parameter[,…]]) ;
```

### 14.2.2、调用存储函数

存储函数的使用方法与MySQL内部函数的使用方法是一样的


## 14.3、查看存储过程和函数

```sql
SHOW { PROCEDURE | FUNCTION } STATUS [ LIKE 'pattern' ] ;

SHOW CREATE { PROCEDURE | FUNCTION } sp_name ;

SELECT * FROM information_schema.Routines WHERE ROUTINE_NAME=' sp_name' ;
```

## 14.4、修改存储过程和函数

```sql
ALTER {PROCEDURE | FUNCTION} sp_name [characteristic ...]
characteristic:
{ CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
| SQL SECURITY { DEFINER | INVOKER }
| COMMENT 'string'
```

## 14.5、删除存储过程和函数

```sql
DROP { PROCEDURE| FUNCTION } sp_name;
```