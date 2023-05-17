---
date: 2021-09-30 16:57:30
title: 马尔可夫模型

---
# 马尔可夫模型
## 马尔可夫过程/马尔可夫链
马尔可夫过程是满足*无后效性*的随机构成

马尔可夫链是离散的马尔可夫过程。

马尔可夫链定义如下：

## MRP（马尔可夫奖励过程）
- Return（奖励）
- Discount
- Value Function（估值函数）
- Bellman Equation
  
  $$
  
  v = \mathcal{R} + \gamma \mathcal{P}v
  
  $$

  Bellman is linear, can be solved in $O(n^3)$ by Guassian Elimination or iterative method

## MDP（马尔可夫决策过程）
- Action
- Policy
- Value Function
- Action-Value Function
  
  $$
    q_{\pi}(s,a) = \mathbb{E}_{\pi}[G_t|S_t = s, A_t = a]
  $$

- Bellman Expectation Equation
- Value Function vs. Q-Function
  
  $$
    v_{\pi}(s) = \max_{a} q_{\pi}(s,a)
  $$

- Bellman Optimality Equation
  - Bellman Optimality Equation is non-linear
  - **No closed form**
  - Many iterative solution methods
    - Value Iteration
    - Policy Iteration
    - Q-Learning