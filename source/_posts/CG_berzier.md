---
title: 贝塞尔曲线
abbrlink: cc351245
date: 2021-10-29 02:08:09
---
# 贝塞尔曲线
## 性质
- 端点性质
- 切性质
- 非负性
- 递推性
- 凸包包围
- 对称性
- 仿射不变性

## de Casteljau 算法（待完善）
利用线性插值而不是幂级数来计算贝塞尔曲线插值

## 贝塞尔曲线的缺陷
没有局部性：改变一个控制点，会影响整个曲线

## 分段贝塞尔曲线

其实是样条的思想。

由于是样条，所以每两个点之间存在一条贝塞尔曲线（2阶或3阶），而且可以过每个给定点

分段贝塞尔曲线一定程度上解决了全局性，但维护光滑条件有难度

### 光滑条件

- C0：曲线连续
- C1：切线连续
- C2：曲率连续

- G0：同C0
- G1：公共切线（弱于C1）
- G2：公共曲率圆（弱于C2）

## B样条（待完善）

**B样条不等于贝塞尔样条**