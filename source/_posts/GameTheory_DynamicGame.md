---
date: 2021-12-09 14:22:07.984555+08:00
title: Dynamic Game

---
# Dynamic Game
## 与Static Game的区别
Static Game强调**同时做出决策，或决策的先后对结果没有影响**，典型的如猜拳。

Static Game中认为Player不会讲自己的决策预先告诉对方，比如猜拳不会先告诉对方自己出什么。

Dynamic Game区别于Static Game的本质是**Player在不同阶段做出决策时，他们的已知不是0而是动态的**

如下这种表达说明了区别：
- Static Game其实是multi-player simultaneous decision
- Dynamic Game是混合了seqential decision和simultaneous decision的Game

很多时候Dynamic Game存在所谓的*先发优势*：首先决策的人能引导整个游戏一开始就走向有利于自己的方向。

## Backward Induction

解决Dynamic Game的朴素方法。

也就是从Game Tree逆推Player的决策。这是因为在Game Tree的每个“最后一步”，我们都可以直到Player已知的前置信息，此时游戏退化为简单的决策问题。

在这样的线性决策中，**Player总是贪心地决策当前局面下的最优动作**。然后，**剪除这一决策点，选择的结果代替决策节点**

修剪的结果是一个博弈的均衡点。可以证明这一（behavioral strategy的）均衡点就是NE

**单纯的Backward Induction只适用于只含有sequential decision的game tree**

## Strategy

在Static Game中，Strategy退化为Action同等的概念。

完整的Pure Strategy概念是：**关于给定当前状态到所采取的Action的映射**

## NE of Dynamic Game

利用Pure strategy概念，可以将Dynamic Game等价转换为一个Static Game。

此时，可以利用Static Game的方法求解Dynamic Game的NE（如Pure strategy的Best Response）

但是，**这样得到的NE不能反映Dynamic Game的本质和均衡点。因为Dynamic Game存在决策的顺序，策略的均衡点也许在行为上不可行或不现实**

## Information Set

定义这样一些节点属于同一个Information set：
- 节点属于同一个Player
- Player不知道会到达集合中的哪一个节点

Information Set用于**在sequential decision中混入（表征）simultaneous decision**

可以通过“将两个节点划分在同一个Information Set中”来表达“其实决策是simultaneous而不是sequential的”。因为如果是sequential的，那这两个节点就不在同一个Information Set中了。

## Behavioral Strategy
两种定义随机化策略的方法：
- Mixed strategy：认为是关于（全局）Pure strategy的概率分布
- Behavioral strategy：认为是在每个Information Set下局部的关于采取的Action的概率分布

不但可以证明Behavioral strategy和Mixed strategy存在对应，**Behavioral strategy意义下的NE也可以对应到Mixed strategy下的NE**

## Subgame Perfection
- Subgame
- Subgame Perfect NE
  - 定义：**Subgame Perfect NE指明的behaviour在任何subgame中仍然是NE**（即便这个subgame在实际决策中不会涉及）
  - 也即Subgame Perfect NE是比NE更强的点，是NE的子集
  - 定理：任何finite dynamic game都存在subgame perfect NE