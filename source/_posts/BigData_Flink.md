---
date: 2021-11-29 17:48:00.336707+08:00
title: Flink

---
# Flink
- 架构
  - Source 流数据源
  - Transform 流处理
  - Sink 流数据输出
- API：
  - Datastream 流处理
  - Dataset 批处理
- 窗口：
  - 翻滚窗口
  - 滑动窗口
- 时间：
  - 物理时间
  - 事件时间
- 水位线：为了解决乱序问题。要求当前到达Time Stamp超出窗口结束点一定“水位”才结束统计。