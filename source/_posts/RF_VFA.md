---
date: 2021-10-21 14:21:48
title: Reinforcement Learning with Value Function Approximation
---
# Reinforcement Learning with Value Function Approximation

Value Function Approximation(VFA)

## Whey value function approximation
- 大规模决策问题
  - 状态空间极大：国际象棋，围棋
  - 连续取值问题

## VFA. vs. Q-Learning
- SARSA/Q-Learning本质上都是**基于一个Q值表做估值和预测**
- Value function approximation**不试图建表存储所有输入的Q值**，而是**直接从输入近似计算Q值**

## TD/MC与VFA结合（待完善）
利用TD/MC估计梯度

## VFA Control
VFA只是估计梯度，控制还是需要SARSA/Q-Learning