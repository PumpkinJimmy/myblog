---
title: 分布式哈希表（DHT）
abbrlink: af077b40
date: 2021-11-10 08:35:20
---
# 分布式哈希表（DHT）
## 简介
考虑将经典哈希表中的存储桶换成*分布式存储节点*。

我们使用哈希值来命名一个*entity*，同样用哈希值来命名一个*node*。

则我们规定：**一个名为$k$的entity存储在名为$succ(k)$的node中**（其中$succ(k)$表示$k$的后继）

注意，不像传统的哈希表，**哈希值$h$对应的桶不一定存在（对应存储节点不一定存在）**。因此需要存放在$succ(k)$而不是k。

最后，将哈希函数的值域全域按序排列，然后收尾相连成环。在这个环中，每个节点只掌握相邻节点的访问信道。这样的结构就是经典的*Chord*

显然，分布式系统的特性决定了DHT不再具有$O(1)$性能。因为桶的访问不但不是随机访问的，还依赖一定的路由才能通过网络访问到。因此，在没有额外的信息的情况下，只能沿着环顺序查找。

因此，我们需要在每个节点上维护额外一张关于另外一些节点访问地址的表。这张表就是*Chord Finger Table*

这个表实际上是变式的ST表。

## 特点
1. 这种简单模式的出众之处在于**这是一种真正的去中心的P2P网络**。
2. 这种分布式存储系统非常易于拓展

## 问题
1. 性能不如集中式的存储系统
2. 新节点加入和删除的时候可能导致不一致
3. 负载均衡问题
4. 地理问题：哈希值上相邻的节点可能在地理上不相邻。可能导致过大的网络开销。