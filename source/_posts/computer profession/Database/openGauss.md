---
title: openGauss
tags:
  - 专业课
abbrlink: d49e7214
date: 2021-08-31 11:11:18
updated: 2021-08-31 11:11:18
---
# openGauss

注意安装好之后需要reboot或者source一下环境变量的变更才能生效。

注意官方文档的说明是有问题的。在极简版下根本没有`gs_om`指令。管理openGauss应该使用`gs_ctl`的系列CLI：
```bash
# 启动数据库服务
gs_ctl start -D [DATA_DIR] -Z single_node 
# 查询数据库服务状态
gs_ctl status -D [DATA_DIR]
```

注意指令里的`-D`省略的话会默认使用`$PGDATA`环境变量的值

连接数据库`gsql`
```bash
gsql -d dbname [-h host] [-p port] [-u user] [-W passwd]
```

**导入数据**：由于这玩意通常没有现成的支持格式，所以最简单粗暴的做法是直接使用*csv*来导入数据。因为所有的主流SQL数据库都支持生成csv脚本。

注意，**openEuler其实是CentOS基础上改过来的**，**openGauss实在PostgreSQL基础上改过来的**。这使得资料有一定的通用性

## 导入导出CSV

使用`Copy`命令。注意，CSV格式一次只能导入一个表。