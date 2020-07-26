<!-- # 第 1 章 数据库概述 -->

## 1.1、数据存储方式

- 人工管理阶段
- 文件系统阶段
- 数据库系统阶段

## 1.2、数据库泛型

数据库泛型就是数据库应该遵循的规则。数据库泛型也称为范式。目前关系数据库最常用的四种范式分别是：

第一范式（1NF）、第二范式（2NF）、第三范式（3NF）和BCN范式（BCNF）

## 1.3、SQL语言

SQL（Structured Query Language）语言的全称是结构化查询语言。数据库管理系统通过SQL语言来管理数据库中的数据。

SQL语言分为三个部分：

- 数据定义语言（Data Definition Language，简称为DDL）

```sql
CREATE TABLE …
```

- 数据操作语言（Data Manipulation Language，简称为DML）

```sql
SELECT … 
INSERT …
UPDATE …
DELETE …
```

- 数据控制语言（Data Control Language，简称为DCL）。

```sql
GRANT …
REVOKE …
```

## 1.4、为什么要使用MySQL

1. MySQL是开放源代码的数据库
2. MySQL的跨平台性
3. 价格优势
4. 功能强大且使用方便

## 1.5、常见数据库系统

1. 甲骨文的Oracle
2. IBM的DB2
3. 微软的Access和SQL Server
4. 开源PostgreSQL
5. 开源MySQL
6. 文件数据库SQLite,
7. 内存数据库HQL
