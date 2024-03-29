---
title: 以太坊：区块链2.0
abbrlink: cb0efc5f
date: 2021-10-28 10:04:04
---
# 以太坊：区块链2.0


## 智能合约

传统合约的特点：
- 合约生效到合约执行可能存在时间差
- 传统合约依赖可信任的公证人
- 如果存在违约，需要依赖政府、法院来强制执行

智能合约的特点：
- 事件驱动
- 价值转移
- 自动执行

### 应用于区块链上的智能合约
- 区块链本质上是一个激励驱动的分布式共识系统
- 区块链的透明性、多方验证、不可篡改特性非常适合于智能合约

## 以太坊简介

对比特币的一些改进：
- 出块时间15s，大幅减少交易延迟
- 挖矿谜题对内存要求高，从而限制ASIC的使用
- 为了将使用PoS替代PoW

另外，以太坊最突出的新特性是**智能合约**

- 以太坊支持智能合约，使得区块链有了更广阔的应用（而不仅仅是发币）
- 以太坊促进了DApp(Distributed App)的发展

### 对比比特币
- 支持智能合约
- 增加叔块(Uncle Block)奖励，减少出块实践
- Ghost共识
- 更加活跃的开源社区

### 典型应用
- 溯源存证
- 数字资产发行与流通
- 数据共享
- 游戏

## 以太坊账户结构
- 地址（公钥哈希得）
- 余额
- 主动发起交易得次数（写作Nounce）

注意以太坊账户会记录余额，不需要UTXO算

### Nounce的作用
其实是为了编号某用户发起的交易，以防范*重放攻击*

### 两种账户
- 通常账户（外部账户）
- 合约账户
  
  有代码的账户，能触发执行智能合约。

  有额外的代码哈希和存根地址

### 对比UTXO
UTXO不存储账户余额，理论上隐私保护更好。

但为了支持（智能合约相关）额外功能，以太坊不希望用户能任意地创建新身份。

## 交易数据结构（待完善）
交易：
- 通常的交易
  - from
  - to
  - 金额
- **智能合约**
  - from
  - to
  - value（金额）
  - gasPrice（手续费）
  - **data**
    
    这里面包含了智能合约的用户数据和**程序**

  - **gasLimit**
  
    合约可用的gas的上限。这一上限的设置是为例**防止合约程序不停机**
EVM：执行智能合约代码的虚拟环境。

注意智能合约的代码除了直接来源于data，还可能来源于“引用另一个交易中的data”

## 以太坊的分叉
- 以太网的出块时间是15s，因此会频繁分叉
- 如果按照传统的比特币“最长链”原则去认证和奖励出块。这意味着**在分叉中不被认可的块得不到任何出块奖励**
- 在以太坊频繁分叉的背景下，这样的奖励机制会打击矿工的积极性

解决方案就是**奖励叔块**

### 叔块
- 每个区块至多接受两个叔块
- 叔块不能重复获得奖励
- 随着距离越远，叔块的奖励越来越少
- **叔块的交易不被执行**