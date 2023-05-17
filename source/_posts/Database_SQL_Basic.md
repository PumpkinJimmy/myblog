---
abbrlink: fd62bcd9
categories:
- 数据库
date: 2021-09-13 10:06:52
tags:
- 数据库
- SQL
- 专业课
title: 基础SQL

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

可以把`values`子句改为嵌套子查询

### delete from
```sql
delete from table1 where cond1
```

`where`可以嵌套子查询

### update
```sql
update table1
set col1 = val1
where cond1
```

`where`可以嵌套子查询

### 使用case（待完善）

## SQL 杂项
### 别名 as

若查询中涉及同一关系模式的多个关系实例参与运算，则不能使用表名指代表，需要**显式为关系实例指定别名**

```sql
select T.attr1
from table1 as T, table1 as S
where T.attr1 > S.attr1
```

`as`也可以用在列名部分（如在聚集函数中）：
```sql
select avg(col1) as avg_col1
from table1
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

## 空值

### 空值判别
`where`子句中**使用is null而不是= null来判别取值是否为null**。

原因是`null = null`的取值是`unknown`而不是`true`（见下文）

### 空值算术运算

规定对`null`的算术运算的结果都是`null`

### 三值逻辑

空值导致比较谓词存在陷阱：

若令`x < null`为`false`，意味着`not(x < null)`为`true`，这一般都不是想要的。

为了处理空值的谓词逻辑，需要对二值的布尔逻辑做扩充：
- `true`
- `false`
- `unknown`（新增）

`unknown`主要出现在**涉及null的谓词中**。

`unknown`基本演算：
$$
    true \land unknown = unknown\\
    false \land unknown = false\\
    unknown \land unknown = unknown\\
$$

$$
    true \lor unknown = true\\
    false \lor unknown = unknown\\
    unknown \lor unknown = unknown\\
$$
$$
    \lnot unknown = unknown
$$

我们规定，**任何关于null的比较运算，结果都是unknown**

这样的规定保证了`x < null`和`not (x < null)`都不为真

注意，`where`子句的条件若为`unknown`，则当作`false`处理



## 聚集

所谓*聚集*，相比于常规操作，聚集操作是**关于值集合的多对一映射过程**。常见的求平均、计数，其实可以是聚集过程。

### 聚集函数
```sql
avg
min 
max
sum
count
```
**小心，SQL select 默认不去重，count尤其可能出错**

使用如下形式做不重复的计数：
```sql
select count(distinct col1)
from table1
```

### groupby
一种常见的需求是*分组聚集*。如，相比于统计全公司平均薪酬，我们更关心不同部门的平均薪酬。此时我们的需求是“按所属部门分组，在分别聚集”。

语法如下：
```sql
select col1, avg(col2)
from table1
where cond1
group by col1
```

要点：
- `group by`指明分组依据的列。对应列取值相同的行会分为同一组
- 在聚集中，`select`子句中出现的属性必须是*聚集属性*
- 所谓*聚集属性*是指
  - 套着聚集函数的属性
  - `group by`属性


### having（待完善）
having和

### 空值聚集
- 聚集都会自动忽略空值
- count(*)例外，即便一个元组是全null值的，它也会算上
- 若属性取值集合全是null值，则：
  - count返回0
  - 其他函数返回null

## 嵌套子查询
### where 嵌套子查询
在`where`子句中嵌套子查询的主要动机是为利用查询结果集来执行灵活的谓词运算。
- 成员资格测试
  
  其实就是`in/not in`

- 集合元素比较（量词）
  
  支持`>/</=/>=/<=/<>`集合的`some/all`

- 空关系测试

  `exists/not exists`，配合集合运算有奇效

- 重复性测试
  
  `unique`，在集合无重复元组时为真，否则为假

### from 嵌套子查询
一般认为`select`返回得结果是一个元组结合，因此也可以放入from

通常外层查询中可以使用内层的相关变量，`lateral`子句使得内层可以使用外层的变量（很少有支持）

### with 子句
方便的耍赖手段。利用`with`子句，可以将一个查询的结果表示为一个临时的变量，在后续引用。
```sql
with max_budget(value) as
  (select max(budget) from department)
select department_name
from department, max_budget
where department.budget = max_budget.value;
```

### 标量子查询
实际上，很多查询的结果实质上是一个标量，但由于关系运算的封闭性，标量结果会变成单行单列的关系。

如果SQL用户确信某个查询的结果就是一个标量，可以直接将嵌套子查询当作一个标量用在`where`子句甚至是`select`子句中。

滥用标量子查询的问题：
- 标量子查询破坏了SQL语句所映射的关系代数运算的封闭性
- 一个子查询的结果是否标量在编译阶段是无法检查的，因此一个错误将非标量子查询当作标量使用的语句会触发运行时错误