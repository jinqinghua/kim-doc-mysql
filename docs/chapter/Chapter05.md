<!-- # 第 5 章 操作数据库 -->

假设已经登录

```sql
mysql -h localhost -uroot -proot
```

## 5.1、显示、创建、删除数据库

```sql
show databases;      -- 显示所有的数据库
create database xxx; -- 创建数据库
drop database xxx;   -- 删除数据库
```

## 5.2、数据库存储引擎

```sql
show engines \G                 -- mysql支持的所有的engine
show variables like '%engine%'; -- 查看当前库的engine
```

1. InnoDB

    - 最常用，支持事务，回滚，自增，外键
    - 表结构存在.frm文件中
    - 数据和索引存在表空间中
    - 读写效率稍差，占用空间大

2. MyISAM

    - 表结构存在.frm文件中
    - .myd存储数据
    - .myi存储索引
    - 快速，占空间小，不支持事务和并发
  
3. Memory
