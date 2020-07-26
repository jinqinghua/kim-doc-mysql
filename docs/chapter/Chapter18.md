<!-- # 第18章 性能优化 -->

## 18.1、优化简介

```sql
SHOW STATUS LIKE 'value';

connections     -- 连接数
uptime          -- 启动时间
slow_queries    -- 慢查询次数
com_select      -- 查询操作次数
com_insert      -- 插入操作次数
com_update      -- 更新操作次数
com_delete      -- 删除操作次数
```

## 18.2、优化查询

### 18.2.1、分析查询语句

```sql
EXPLAIN/DESC select;

type            -- 连接类型
system          -- 表中只有一条记录
const           -- 表中有多条记录，但只从表中查询一条记录
all             -- 对表进行了完整的扫描
eq_ref          -- 表示多表连接时，后面的表使用了unique或PRIMARY KEY
ref             -- 表示多表查询时，后面的表使用了普通索引
unique_subquery -- 表示子查询中合作了unique或primary key
index_subquery  -- 表示子查询中使用了普通索引
range           -- 表示查询中给出了查询的范围
index           -- 表示对表中索引进行了完整的扫描
possible_key    -- 表示查询中可能使用的索引
key             -- 表示查询时使用到的索引
```

### 18.2.2、索引

1. 走单列索引
2. 走多列索引
3. 不走索引的查询
   - Like 以 %开头的不走
   - Or 两边的列有一个没有建立索引不走索引
   - 多列索引第一个字段没有使用不走索引

## 18.3、优化数据库结构

### 18.3.1、将字段很多的表分解成多个表

### 18.3.2、增加中间表

### 18.3.3、增加冗余字段

反范式

空间换时间

### 18.3.4、优化插入记录的速度

1、禁用索引

```sql
ALTER TABLE table DISABLE/ENABLE KEYS;
```

2、禁用唯一索引

```sql
SET UNIQUE_CHECK=0/1
```

3、优化INSERT语句

```sql
使用 INSERT INTO table (f1,f2….fn) VALUES 
(v1,v2….vn),
(v1,v2….vn),
(v1,v2….vn),
…
(v1,v2….vn);
```
代替多个INSERT INTO

### 18.3.5、分析、检查和优化表

```sql
ANALYZE TABLE table1[, table2…]
CHECK TABLE table1[, table2…]
OPTIMIZE TABLE table1[, table2…]
-- 优化文本字段，消除更新操作带来的碎片，减少空间浪费
```

## 18.4、优化MySQL服务器

### 18.4.1、优化服务器硬件

- CPU
- 磁盘，阵列
- 内存
- 配置（专用服务器，大内存配置）

### 18.4.2、优化MySQL参数
