---
date: 2021-11-15 08:35:29.885881+08:00
title: 流计算

---
# 流计算
## Storm
### 基本概念
- Tuple
  
  可以认为是“行”。

  更精确地说，Tuple的意义是一系列Map的ValueList

- Spout
  
  流数据源，理解为“水龙头”

- Bolt
  
  计算组件。

  他的输入是Tuple流，输出也是Tuple流

### Topology

我们将Spout和Bolt作为节点可以构建出一个流计算图Topology。

Bolt通过*订阅*得到Spout的数据。因此中间会涉及通信中间件。

Topology是Job的最小单位。

### Grouping
- ShuffleGrouping 随机分组
- FieldsGrouping 按字段取值分组
- AllGrouping 广播发送，Tuple发往所有后继
- NonGrouping 不分组，后继全部在同一个进程


