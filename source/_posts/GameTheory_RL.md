---
title: Model-free Reinforcement Learning
abbrlink: 6cab2481
date: 2021-10-14 14:37:30
---
# Model-free Reinforcement Learning

## MDP的局限
在现实决策问题中，MDP存在诸多限制

- 状态空间可能非常大甚至无限大，不可能直接应用算法求解Q函数
- 环境可能不完全可知（随机，或不完全信息博弈），换句话说转移概率可能根本不可知，或者当前不可知

## Monte-Carlo Policy Evaluation

利用蒙特卡洛做采样，可以估计给定策略的效用函数值。

通常只适用于有限MDP。

简单的频率作为概率的估计的方法。

蒙特卡洛为了解决传统MDP求解的困难，采用的是移动指数平均值来实现在线更新。

## Temporal Difference Learning

将蒙特卡洛方法继续推进到极端，**只采样眼前一步的结果更新估值**。

游戏的模型越是符合马尔可夫条件，该方法越有效。

## 综述基本强化学习策略
按照对状态-动作搜索树的搜索策略，分为以下四大类策略：
- Temporal Difference 对标贪心
- Monte-Carlo 对标DFS
- Dynamic Programming 对标BFS
- Policy Iteration / Value Iteration 对标穷举