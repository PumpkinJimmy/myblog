---
date: 2021-12-22 17:02:35.738019+08:00
title: PBFT

---
# PBFT

PBFT 全称*Practical Byzantine Fault Tolerance*，实用拜占庭容错

**PBFT在允许任意类型失效（包括恶意）的几乎异步的3k+1节点系统中提供k容错**

主要分为5个阶段：
1. Request: Client向Primary节点发送请求
2. Pre-prepare: Primary多播消息
3. Prepare: 除了Primary之外每个Replica多播PREPARE消息
4. Commit: 每个节点再多播一次消息
5. Replay : 所有节点回复Client

注意，Pre-prepare - Prepare - Commit 类似经典的3PC协议

大部分正确性证明都依赖Quorum（交集）来证明

## Detail

### Prepare
- 多播`PREPARE(view, seq#, digest)`消息
- **当收到$2k$个匹配的PREPARE消息，则构建一个P-certificate，并进入Commit阶段**

### Commit
- 发送`COMMIT(view, seq#, digest)`消息
- **当收到$2k+1$个匹配的COMMIT消息，则构建一个C-certificate(msg, v, seq#)，并正式执行操作**
- 该算法能够对抗干扰的逻辑是：如果收到的匹配的commit不够多，那就不能提交

### Replay
- 所有节点向Client发送确认消息


## View Change
三步：
- 多播发起View Change
- 新Primary收集消息，收齐确认之后多播New View消息
- 收到New View消息之后，各个节点还需要交换消息