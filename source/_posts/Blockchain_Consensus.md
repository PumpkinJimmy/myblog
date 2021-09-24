---
date: 2021-09-23 11:12:09
---
# 区块链共识机制
## 共识的定义
A process of agreement between distrusted nodes on the final state of specified data.

### 说明
定义中的关键字：
- 共识是*达成一致*的过程
- 共识是针对*相互不信任的节点*之间的操作（这暗示了运用共识的常见场景就是分布式系统）

## 一致性
### 一致性的要求
（待完善）

### 一致性的问题和挑战
- 网络是不可靠的
- 节点的处理时间是没有保障的
- 节点可能是恶意的
- 同步调用会严重降低分布式系统的可拓展性

### 可能的解决方案
- 同步调用
- 提前约定
- 第三方中心调度（不适合去中心化的区块链）

## 共识机制
### 定义
A set of steps taken by most/all nodes in a distributed system(e.g. blockchain) to agree on a proposed state or value

### 要求
- Agreement 一致性（基本要求）
- Termination 可终止性
- Validity 合法性：鉴别提案节点的身份
- Fault Tolerance 容错性
- Interity 完整性

### 主要类别
- BFT-based 基于拜占庭容错的共识算法
  - 性能较高
  - 只使用在小规模的系统
  - 可拓展性差

- Leader Election-based 基于选举的
  - “赢家通吃”：只有最终的胜利者能够提出最终取值
  - 可拓展性很好
  - 性能很低

## 区块链共识机制
- 公链
  - 几乎可以加入任何节点
  - 参与者完全不存在信任
  - 使用算力敏感的PoW机制
  - 代表项目是比特币
- 联盟链/私链
  - 节点加入系统需要审查，节点之间具有信任基础
  - 节点较少
  - 可以使用BFT-based的高效共识算法

## 拜占庭容错
拜占庭容错（BFT）来源于拜占庭将军问题：
- 拜占庭的将军们分布在各地
- 关于是否开战的问题需要将军们达成共识
- 在通信过程中可能有外敌干扰

为解决拜占庭将军问题而设计的策略就是*拜占庭容错(BFT)*