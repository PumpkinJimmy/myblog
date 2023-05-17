---
abbrlink: '0'
date: 2021-09-22 14:33:53
title: 数据库权限系统

---

# 数据库权限系统
## 主要类别
数据库中常见的权限：
- 对于表数据
  - 增 Insert
  - 删 Delete
  - 改 Update
  - 查 Select
- 对于表
  - 增加表 Create
  - 删除表 Drop
  - 修改表 Alter
  - 索引 Index

## Grant 授权

授权的基本操作

```sql
grant <privilege list>
on <relation/view>
to  <user list>
```

说明：
- 视图及其关联的表**不共享权限**，也就是能插入视图不代表能插入原表
- `<user list>`可以是`public`，意为“当前及未来的所有有效用户”
- `<privileges list>`可以是`all privileges`，意为“一切权限”

举例：
```sql
grant select on some_table to U1
```

## 回收权限
```sql
revoke <privilege list>
on <relation/view>
to  <user list>
```

回收权限的问题：
- **若一个权限被多次授予同一个用户（常见于由多个DBA授予同一个用户），则一次`revoke`操作可以回收权限一次**
- 某些情况下，权限可能被级联回收

## Role 角色（用户组）（待完善）
可以创建role用户实现成组的用户管理

## 其他权限
外键权限：
```sql
grant reference (some_col) on some_table to some_role;
```

## 权限转移
当在授权的时候带上`with grant option`，就允许了**被授权用户可以将自己持有的这一权限授予用户**。

有了权限转移，所有用户的权限可以表示为一个有向图模型，其中包含一些授权链。

成链的权限默认是*级联回收*的。也即`revoke`可以沿着授权链，将一个用户及其授予他人的相应权限全部收回。

`revoke`可以只收回`with grant option`而不收回任何具体权限