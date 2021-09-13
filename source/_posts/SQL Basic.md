---
title: 基础SQL
date: 2021-09-13 10:06:52
categories: 
- 数据库
tags:
- 数据库
- SQL
- 专业课
---

# 基础SQL
## SQL DDL
```sql
create table table_name (
    col1 col_type1,
    col2 col_type2 not null,
    primary key(col2),
    foreign key(col1) references table2,
    check(col1 < some_val)
);

alter table table_name add/alter/delete column my_col col_type

drop table

create index on 

drop index
```

## SQL DML
### select
``` sql
select [distinct] col1, col2, ...
from table1, table2, ...
where cond1, cond2, ...
```

### natural join
```sql
select some_col
from table1 natural join table2 natural join table3
```

### insert into
```sql
insert into table1 values (val1, val2, ...)
```

### delete from
```sql
delete from table1 where cond1
```

### update
```sql
update table1
set col1 = val1
where cond1
```

## SQL 杂项
### 别名 as

若查询中涉及同一关系模式的多个关系实例参与运算，则不能使用表名指代表，需要**显式为关系实例指定别名**

```sql
select T.attr1
from table1 as T, table1 as S
where T.attr1 > S.attr1
```

### 字符串
#### 引号问题
SQL字符串是单引号定界的。若字符串中包含单引号，使用连续两个单引号表示一个单引号。
#### 字符串函数
```sql
upper
lower
trim
|| # 串联（拼接）
```
#### 字符串匹配


模板语法
- `_`匹配一个字符
- `%`匹配任意长度的子串

有了模板，使用`like`子句在`where`中使用：
```sql
select some_col
from table1
where col1 like 'template' [escape] '\'
```
注意`escape`用于指定转义字符

### 显示的次序
严格来说，关系中的元组是无须的，因此`select`语句的返回结果也不保证顺序。

`order by`子句可以指明`select`返回顺序：
```sql
select some_col 
from table1
order by col1, col2 [desc/asc]
```


### where谓词语法
- 元组比较
  ```
  select some_col
  from table1
  where (col1, col2) = (val1, val2)
  ```
- `between`
  ```
  select some_col
  from table1
  where col1 between lb and ub
  ```

## 集合运算
### 不重复（默认）
```sql
union # 并
intersect # 交
except # 差
```
### 允许重复的集合运算
```sql
union all # 并，重复元组出现次数会求和
intersect all # 交，保留两个原集合中较小的一个出现次数
except all # 差，出现次数详见，但不小于0
```

## 聚集

### 聚集函数

### groupby


## 嵌套子查询