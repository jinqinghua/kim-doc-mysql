<!-- # 第 7 章 索引 -->

MySQL中，所有的数据类型都可以被索引，包括普通索引，唯一性索引，全文索引，单列索引，多列索引和空间索引等。

## 7.1、索引简介

### 7.1.1、索引的含义和特点

BTREE 索引，HASH 索引

优点：提高查询，联合查询，分级和排序的时间

缺点：索引占空间，维护（创建，更新，维护）索引时需要耗费时间

### 7.1.2、索引的分类

1. 普通索引：不加任何限制条件
2. 唯一性索引：使用UNIQUE参数
3. 全文索引：使用FULLTEXT参数，只能创建在CHAR,VARCHAR,TEXT类型的字段上，只有MyISAM存储引擎支持全文索引。
4. 单列索引：在一个字段上建立的普通索引，唯一性索引或全文索引
5. 多列索引：在多个字段上建立的普通索引，唯一性索引或全文索引
6. 空间索引：使用SPATIAL参数，只有MyISAM存储引擎支持空间索引，必须建立在空间数据类型上，且必须非空，初学者很少用到。

### 7.1.3、索引的设计原则

1. 选择唯一性索引
2. 为经常需要排序、分组和联合操作的字段建立索引。如ORDER BY、GROUP BY、DISTINCT，UNION等操作的字段，特别是排序
3. 为常作为查询条件的字段建立索引
4. 限制索引的数目，避免过多地浪费空间
5. 尽量使用数据量少的索引
6. 尽量使用前缀来索引
    - 如指索引TEXT类型字段的前N个字符(
    - Oracle中有函数索引，这个是不是相当于left(field, n)式的函数索引？！

7. 删除不再使用或者很少使用的索引

## 7.2、创建索引

三种方式：

1. 创建表时创建索引

2. 已经存在的表上创建索引

3. 使用ALTER TABLE语句来创建索引

### 7.2.1、创建表的时候创建索引

```sql
CREATE TABLE 表名 (
    属性名 数据类型 [完整约束条件],
    属性名 数据类型 [完整约束条件],
    …
    [UNIQUE|FULLTEXT|SPATIAL INDEX|KEY [别名] (属性名1 [(长度)] [ASC|DESC])
);
```

#### 1、创建普通索引

```sql
CREATE TABLE index1 (
    id INT,
    name VARCHAR(20),
    sex BOOLEAN,
    INDEX(id)
);

SHOW CREATE TABLE index1\G;
```

#### 2、创建唯一性索引

```sql
CREATE TABLE index2(
    id INT UNIQUE,
    name VARCHAR(20),
    UNIQUE INDEX index2_id(id ASC)
);

SHOW CREATE TABLE index2\G;
```

看到在字段 id 上建立了两个唯一索引 id 和 index2_id，当然这样是没有必要的。

#### 3、创建全文索引

```sql
CREATE TABLE index3 (
    id INT,
    info VARCHAR(20),
    FULLTEXT INDEX index3_info(info)
) ENGINE=MyISAM;
```

#### 4、创建单列索引

```sql
CREATE TABLE index4 (
    id INT,
    subject VARCHAR(30),
    INDEX index4_st(subject(10))
);
```

注意：只索引subject前10个字符

#### 5、创建多列索引

```sql
CREATE TABLE index5 (
    id INT,
    name VARCHAR(20),
    sex CHAR(4),
    INDEX index5_ns(name, sex)
);

EXPLAIN select * from index5 where name='123'\G;
EXPLAIN select * from index5 where name='123'and sex='N'\G;
```

#### 6、创建空间索引

```sql
CREATE TABLE index6 (
    id INT,
    Space GEOMETRY NOT NULL,
    SPATIAL INDEX index6_sp(space)
) ENGINE=MyISAM;
```

### 7.2.2、在已经存在的表上创建索引

```sql
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX 索引名 ON 表名 (属性名[(长度)] [ASC|DESC]);
```

#### 1、创建普通索引

```sql
CREATE INDEX index7_id on example0(id);
```

#### 2、创建唯一性索引

```sql
CREATE UNIQUE INDEX index_8_id ON index8(course_id);
```

#### 3、创建全文索引

```sql
CREATE FULLTEXT INDEX index9_info ON index9(info);
```

#### 4、创建单列索引

```sql
CREATE INDEX index10_addr ON index10(address(4));
```

#### 5、创建多列索引

```sql
CREATE INDEX index11_na ON index11(name, address);
```

#### 6、创建空间索引

```sql
CREATE SPATIAL INDEX index12_line on index12(line);
```

### 7.2.3、用ALTER TABLE语句来创建索引

```sql
ALTER TABLE 表名 ADD [UNIQUE|FULLTEXT|SPATIAL] INDEX 索引名 (属性名[(长度)] [ASC|DESC]);
```

#### 1、创建普通索引

```sql
ALTER TABLE example0 ADD INDEX index12_name(name(20));
```

#### 2、创建唯一性索引

```sql
ALTER TABLE index14 ADD UNIQUE INDEX index14_id(course_id);
```

#### 3、创建全文索引

```sql
ALTER TABLE index15 ADD INDEX index15_info(info);
```

#### 4、创建单列索引

```sql
ALTER TABLE index 16 ADD INDEX index16_addr(address(4));
```

#### 5、创建多列索引

```sql
ALTER TABLE index17 ADD INDEX index17_na(name, address);
```

#### 6、创建空间索引

```sql
ALTER TABLE index18 ADD INDEX index18_line(line);
```

## 7.3、删除索引

```sql
DROP INDEX 索引名 ON 表名;
DROP INDEX id ON index1;
```