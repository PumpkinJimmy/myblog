---
date: 2021-11-25 14:50:05.008737+08:00
title: Static Game（待完善）

---
# Static Game（待完善）

Static Game: *每个参与者独立、同时作出决策*

## 前言：多智能体博弈
多智能体博弈之所以不能简化为单智能体的优化是因为在多智能体博弈中**博弈的均衡点不一定在对所有人都最好的情况下**，比如囚徒困境。

## 囚徒困境

## 帕累托最优

## Dominant

若对于一个智能体而言，在*任何情况下*一个决策的收益都*大于*另一个的收益，则称这个决策*优于(Strictly Dominant)*另一个决策。

把大于改成大于等于就会得到*Weakly Dominant*

理解：由于每个（纯）决策的效用在不同情况下不同，因此动作的收益可以看作一个向量。我们为向量定义了偏序关系“Dominant”，“Strictly Dominated”意味着一个策略在任何情况下都不是最优的。

Dominant只能用于解决特殊形式的Static Game博弈

## 纳什均衡（NE）

**一个被Strictly Dominated的决策不会是纳什均衡策略**

但Weakly Dominant的策略可能会被包含

对于Pure Strategy，可能不存在纳什均衡

### Pure NE：Best Response

若存在Pure Strategy $(s_1^*, s_2^*)$满足：
- $\pi(s_1^*, s_2^*) \ge \pi(s_1, s_2^*), \forall s_1 \in \mathbf{S_1}$
- $\pi(s_1^*, s_2^*) \ge \pi(s_1^*, s_2), \forall s_2 \in \mathbf{S_2}$

**同时成立**，则$(s_1^*,s_2^*)$是一个Pure Strategy NE

利用这个定理可以快速找出pure strategy NE：只需要找出在每一个Player在对手每一个可能的action下最佳的应对“Best Response”，然后选出那些Best Response重叠的点，就是NE


### Mixed NE: Equality of Payoff

#### 支撑集(Support)的定义
Mixed Strategy $\sigma$的支撑集$S^*$定义为$\{s \ |\ p_{\sigma}(s) > 0\}$


当到达纳什均衡时，**对于每一个Player而言，Support中的任何Pure Strategy的收益等于NE的收益**


可以利用上述的定义来列出等式来求解Mixed-strategy NE。
