---
abbrlink: 6ec1fd6c
categories:
- 算法
date: 2021-09-12 22:19:51
tags:
- 算法
- 算法分析
- 摊还分析
- 并查集
title: 基于启发式合并的O(lglgn)并查集算法

---

# 基于启发式合并的O(lglgn)并查集算法
要点
- 规定一个集合使用深度为2的树表示，集合元素全部是叶子节点
- 树的根节点度不超过$n/lgn$, 每个根节点的子树的孩子数不超过$lgn$
- 查询直接返回祖父
- 合并策略：
  - 启发式。小的并入大的
  - 小的集合中，满$lgn$的子树直接整颗搬到大集合，也就是一棵$lgn$子树只修改一次父指针
  - 不满$lgn$的较小的子树，逐个叶子搬到较大的不满$lgn$子树上，多出来的自成一颗新子树
- 分析：
  - 一个叶子的归属可以被*直接修改*（在不满$lgn$的子树上）和*间接修改*（在满$lgn$的子树上）
  - **一个叶子的归属直接修改的次数为$O(lglgn)$**，因为一个节点被直接修改$k$次意味着其叶子数至少为$2^k \le lgn$，因此$k=O(lglgn)$
  - **一个叶子的归属间接修改的次数平均是$O(1)$**，因为一棵子树的归属至多被改$O(lg(n/lg)) = O(lgn - lglgn)$次，而一颗子数下一共$lgn$个节点，均摊到每个节点上就是$O(1-lglgn/lgn)=O(1)$次
  - 综上，一个叶子平均被修改总次数为$O(lglgn) + O(1) = O(lglgn)$次
- 结论：$n$节点的$m$次并查集操作的总开销是$O(m+nlglgn)$次