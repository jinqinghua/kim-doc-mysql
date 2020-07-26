<!-- # 第 6 章 创建、修改和删除表 -->

## 6.1、创建表

### 6.1.1、创建表的语法形式

```sql
CREATE TABLE 表名 (
    属性名 数据类型 [完整性约束条件],
    属性名 数据类型 [完整性约束条件],
    ......
    属性名 数据类型
);
```

完整性约束条件表：

```sql
PRIMARY KEY      -- 主键
FOREIGN KEY      -- 外键
NOT NULL         -- 不能为空
UNIQUE           -- 唯一索引
AUTO_INCREMENT   -- 自动增加
DEFAULT          -- 默认值

CREATE TABLE example0 (
    id INT,
    name VARCHAR(20),
    sex  BOOLEAN
);
```

### 6.1.2、设置表的主键

- 单字段主键

```sql
属性名 数据类型 PRIMARY KEY

CREATE TABLE example1 (
    stu_id INT PRIMARY KEY,
    stu_name VARCHAR(20),
    stu_sex BOOLEAN,
);
```

- 多字段主键

```sql
PRIMARY KEY(属性名1, 属性名2…属性名n)

CREATE TABLE example2 (
    stu_id INT,
    course_id INT,
    grade FLOAT,
    PRIMARY KEY(stu_id, course_id)
);
```

### 6.1.3、设置表的外键

```sql
CONSTRAINT 外键别名 FOREIGN KEY (属性1.1, 属性1.2,…, 属性1.n) REFERENCES 表名(属性2.1, 属性2.2,…, 属性2.n)

CREATE TABLE example3 (
    stu_id INT,
    course_id INT,
    CONSTRAINT c_fk FOREIGN KEY (stu_id, course_id) REFERENCES example2(stu_id, course_id)
);
```

### 6.1.4、设置表的非空约束

```sql
属性名 数据类型 NOT NULL

CREATE TABLE example4 (
  id INT NOT NULL PRIMARY KEY,
    name VARCHAR(20) NOT NULL,
    stu_id INT,
    CONSTRAINT d_fk FOREIGN KEY(stu_id), REFERENCES example1(stu_id)
);
```

### 6.1.5、设置表的唯一性约束

```sql
属性名 数据类型 UNIQUE

CREATE TABLE example5 (
    id INT PRIMARY KEY,
    stu_id INT UNIQUE,
    name VARCHAR(20) NOT NULL
);
```

### 6.1.6、设置表的属性值自动增加


```sql
属性名 数据类型 AUTO_INCREMENT

CREATE TABLE example6 (
    id INT PRIMARY KEY AUTO_INCREMENT,
    stu_id INT UNIQUE,
    name VARCHAR(20) NOT NULL
);
```

### 6.1.7、设置表的属性的默认值

```sql

属性名 数据类型 DEFAULT 默认值

CREATE TABLE example7(
    id INT PRIMARY KEY AUTO_INCREMENT,
    stu_id INT UNIQUE,
    name VARCHAR(20) NOT NULL.
    english VARCHAR(20) DEFAULT 'zero',
    computer  FLOAT DEFAULT 0
);
```

## 6.2、查看表结构

### 6.2.1、查看表基本结构语句DESCRIBE

```sql
DESC 表名
desc example1
```

### 6.2.2、查看表详细结构语句SHOW CREATE TABLE

```sql
SHOW CREATE TABLE 表名;
SHOW CREATE TABLE example example1\G;
```

## 6.3、修改表

### 6.3.1、修改表名

```sql
ALTER TABLE 旧表名 RENAME [TO] 新表名;
DESC example0;
ALTER TABLE example0 RENAME TO user;
```

### 6.3.2、修改字段的数据类型

```sql
ALTER TABLE 表名 MODIFY 属性名 数据类型;
DESC user;
ALTER TABLE user MODIFY name VARCHAR(30);
DESC user;
```

### 6.3.3~6.3.6、字段及数据类型的增、删，改以及改变位置

```sql
ALTER TABLE 表名 ADD 属性名1 数据类型 [完整性约束条件] [FIRST|AFTER属性名2];
ALTER TABLE 表名 DROP 属性名;
ALTER TABLE 表名 CHANGE 旧属性名 新属性名 新数据类型;
```

利用上面的语句可以增加，删除，修改字段。修改字段名，数据类型，和位置

### 6.3.7、更改表的存储引擎

```sql
ALTER TABLE 表名 ENGINE=INNODB|MYISAM|MEMOERY;
SHOW CREATE TABLE user\G;
ALTER TABLE user ENGINE=MyISAM;
```

### 6.3.8、删除表的外键约束

```sql
ALTER TABLE表名 DROP FOREIGN KEY 外键别名 ;
SHOW CREATE TABLE example3\G;
ALTER TABLE example3 DROP FOREIGN KEY c_fk;
SHOW CREATE TABLE example3\G;
```

## 6.4、删除表

### 6.4.1、删除没有被关联的普通表

```sql
DROP TABLE 表名;
DESC example5;
DROP TABLE example5;
DESC example5;
```

### 6.4.2、删除被其他表关联的父表

删除外键后再删除表

```sql
DROP TABLE example1; -- 报错
SHOW TABLE example4\G;
ALTER TABLE example4 DROP FOREIGN KEY d_fk;
SHOW TABLE example4\G;
DROP TABLE example1;
DESC example1;
```
