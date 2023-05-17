---
title: 大数据计算：MapReduce
abbrlink: cf439812
date: 2021-10-18 16:50:59
---
# 大数据计算：MapReduce
MapReduce提供的是**分布式并行计算**架构

MapReduce指的是
1. Map：分别计算的过程
2. Reduce：归并的过程

MapReduce的核心特性是**封装分布式系统细节，允许用户透明地在分布式系统上编写并行计算高性能程序**

## 基本架构
四个部分：
1. Client
2. JobTracker
3. TaskTracker
4. Task

Task是**计算的基本单元**。两种Task：
- Map Task
- Reduce Task

JobTracker是MapReduce的中枢，它的主要功能：
- **作业调度**
- **资源监控**

TaskTracker的作用是
- 监控管理具体的Task
- 像JobTracker汇报节点存活状况，接受JobTracker指挥

## 基本工作流
```
Map->Shuffle->Reduce
```

- 分片（Split），输入Map中
- Reduce获得各个Map的输出（这一步叫做Shuffle）
- Reduce输出归并的最终输出结果
- 在整个工作流里，Map节点之间互相不通信，Reduce节点之间互相不通信。Map到Reduce的Shuffle和通信全部交给框架完成。

## Split（分片）
Map是独立计算的单元，不负责将输入切片。

切片有专门的过程Split。

**实际上，MapReduce会为每一个Split创建一个Map**

Split的是逻辑概念，其实只是使用索引标记对输入数据的切分。

Split的大小通常与HDFS文件系统块大小有关。

## Map

之所以称分别计算过程为Map，其实是因为这里处理的输入格式必须是*键值映射*。

注意这里的Key不是唯一的。

### Map Shuffle

框架为Map提供一块缓存，存放Map的输出（默认100M）。

当缓存接近满的时候，框架会执行**溢写**，也就是将缓存中的数据写进HDFS文件中

## Combine vs. Merge


