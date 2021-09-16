---
title: 凸优化基础
date: 2021-09-09 14:27:00
categories:
- 最优化
tags:
- 最优化
- 强化学习
- 算法
---
# 凸优化基础
## 定义

若$\mathbb{R}^n$上的点集$C$满足：

$$
    \forall x, y \in C, \theta \in (0,1), \theta x + (1 - \theta) y \in C
$$

则称$C$是*凸集*.

若函数$f: \mathbb{R}^n \to \mathbb{R}$满足：

$$
\begin{cases}
    \mathbf{dom} f\ \text{is a convex set}\\
    f(\theta x + (1-\theta)y) \ge \theta f(x) + (1-\theta)f(y)
\end{cases}
$$

其中$\theta \in (0,1)$，$\mathbf{dom} f$指的是$f$的定义域，则称$C$是*凸函数*.

## 广义拉格朗日

### 定义

### 对偶问题

### KKT条件 & 强对偶定理

（待完善）