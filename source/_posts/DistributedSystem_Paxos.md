---
date: 2021-12-10 15:18:05.742121+08:00
title: Paxos协议

---
# Paxos协议

Paxos是适用于允许节点失效不允许节点恶意行为的分布式共识协议。

系统只要有一半以上的节点是正常工作（未失效）的，系统就可以正常工作。因为这一特点，这个协议也被称为*半数协议*

## 一个共识协议要达到的要求
- **不能对两个不同的值达成共识**
- **在有限步内达成共识**

## 协议假设
- 系统是部分同步的（实践中甚至可以是异步的）
- **节点可以失效，但不可以有恶意**
- 通信可以不可靠（丢失，迟到等），但不可以是伪造的

## 协议详解
- Roles
  - Proposer：发起提案
  - Acceptor：表决提案
  - Learner
- 协议采用*分轮次多数表决*
- 一些符号定义：
  - *rnd* 轮次号
  - *last_rnd* Acceptor目前为止见过的最大的rnd
  - *v* 需要达成共识的对象
  - *vrnd* 对于某个已经表决通过的对象值对应的rnd值
- 协议分为三个阶段（Phrase）
- Phrase 1a
  - Proposer：发送prepare消息，携带rnd和v
  - Acceptor：若收到rnd >= last_rnd，则**接收v**，转入Phrase 1b；否则拒绝rnd < last_rnd的提案值v；
- Phrase 1b
  - Acceptor：向Proposer返回当前vrnd最大的对$(v, vrnd)$，转入Phrase 2
  - Proposer：收集Acceptor们的返回，**选择大多数投票通过的v**
    - 说明：**大多数投票通过的v不一定是Proposer提案的v**，此时基本不是提案的v也必须接受
- Phrase 2（待完善）

## Leader-based Paxos
考虑如果只有Leader能够充当Proposer.

- 基本版本的Paxos可能在Phrase 1 陷入*活锁(Live Lock)*。考虑如果多个Proposer不断地提出rnd更高的消息，系统的节点可能不断地在Phrase 1徘徊。
- Leader-verison的Paxos可以缓解这一活锁的问题，但保证解决活锁。因为**有Leader就要考虑Election，Leader-election本身就是一个共识问题**

## Raft
- Follwer
- Candidate
- Leader

### Election
```
Become candidate -> term++ -> send RequestVote RPCs to others -> Become leader(marjority support) / follower (RPC from leader)
```

### Normal operation - Log Replicating
1. Client send command to Leader
2. Leader add command to its log
3. Leader **sends AppendEntries RPCs to all follower** (inform followers to log the commit)
4. When marjority add log, **command committed**

### （待完善）Log Matching Property

### （待完善）Log Inconsistencies