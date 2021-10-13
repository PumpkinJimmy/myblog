---
title: My New Post
categories:
  - - 算法
    - 数值计算
tags:
  - 数值计算
  - 常微分方程
  - 专业课
mathjax: true
abbrlink: 177574ba
date: 2021-08-22 12:37:43
---
# Ordinary Differential Equation

## Definition

常微分方程是指关于一个未知函数及其任意阶全导数的方程。

所谓*常*微分方程区别于偏微分方程，**不涉及偏导数**

常微分方程的*阶数*表示的是方程中出现的最高阶的导数。

常微分方程有两种常见形式：
1. 关于自变量、未知函数、以及未知函数的方程形式

   通式，基本上就是把定义复述了一遍：
   
   $f(y^{(n)}, y^{(n-1)}, \cdots, y', y, x) = 0$

2. 关于输入函数和输出函数及其导数的方程（常见于系统分析相关的理论）
   
   这种形式实际上是将一个关于输入函数和输出函数的算子表征为一个微分方程的形式。

   此处不涉及这种形式


## 一阶常微分方程

一阶常微分方程理论是相对最完善的。

许多高阶常微分方程都可以变换为一阶常微分组。

**ODE数值方法最基础的就是解一阶常系数微分方程$y' = f(t,y)$**

（也即数值方法不能直接应用在解一般的一阶常系数微分方程$F(t,y,y') = 0$）

## Theorm
- 许多高阶常微分方程都可以变换为一阶常微分组
- 初值问题：给定常微分方程，以及一组初始的取值，求未知函数
- 李普希兹条件：初值问题解存在且唯一
  
  当函数$f(t,y)$关于$y$有**李普希兹连续**的时候，即任取$y$值有：
  $$
   |f(t,y_2) - f(t,y_1)| \le L|y_2 - y_1|
  $$

  该条件的另一个等价形式为：
  $$
   |f_y(t,y)| \le L
  $$

## ODE Method Critiaria

全局末端误差（Final Global Error, F.G.E）是指微分方程数值解在末端点取值与理论解的误差。

## Euler's Method
用于解特定形式的一阶常微分方程（包括线性和非线性的）：
$$
    y' = f(y,t)
$$

（也即不能用于$F(y',y,t)=0$形式的微分方程）

欧拉法**不是迭代法**。因为**微分方程的解是一个函数，

### Error Analysis

Local Error of Euler Method is $O(h^2)$ (from Talyor Expansion)

F.G.E is $O(h)$

区别Local Error和F.G.E：命名都是数值解和解析解之间的误差，为什么末端点数值解与解析解的Local Error是$O(h^2)$，但末端点数值解与解析解之间的误差F.G.E却是$O(h)$？

这是因为：
- **Local Error**是**若前一点是准确值，计算一部的误差**
- 而F.G.E是准确的初值，经过若干步计算，末端的误差
- 每一步都是$O(h^2)$都是



## Henu Method

### Method

Henu Method和Euler Method的共同框架：
$$
   y_{k+1} = y_k + \phi h
$$

欧拉法其实就是令$\phi = f(t_k, y_k)$

Henu Method的改进是改进$\phi$的选择，**采用迭代法选择更好的$\phi$**

初始$\phi$同欧拉法：
$$
   y_{k+1}^0 = y_k + f(t_k,y_k)h
$$

然后取平均，使用新的$\phi$得到一步迭代结果：
$$
   y_{k+1}^1 = y_k + \frac{f(t_k, y_k) + f(t_{k+1}, y_{k+1}^0)}{2} h
$$

反复迭代$n$次到收敛，得到一个数值结果：
$$
   y_{k+1} = y_{k+1}^n
$$

接下类，类似欧拉法，逐步前进即可。

### Error Analysis

Local Error: $O(h^3)$

F.G.E: $O(h^2)$ (much better than Euler Method $O(h)$)

## Midpoint Method

（待完善）

## Runge-Kutta Methods (RK Methods)

Runge-Kutta Methods是一个框架，可以从这个框架导出Euler Method, Heun Method, 还至少可以导出$O(h^2)$到$O(h^6)$的算法.

算法框架核心仍然是：
$$
   y_{i+1} = y_i + \phi h
$$

RK Method 给出了系统的计算$\phi$的方法。

令
$$
   \phi = a_1 k_1 + a_2 k_2 + ... + a_n k_n
$$

称$\phi$为*Weighted Slope*，其中$k$'s有方程组计算：

$$
   \begin{cases}
      k_1 = f(t_i, y_i)\\
      k_2 = f(t_i + p_1 h, y_i + q_{11} k_1 h)\\
      k_3 = f(t_i + p_2 h, y_i + q_{21} k_1 h + q_{22} k_2 h)\\
      \vdots
   \end{cases}
$$

其中$p_i,q_i$都是常数.

常数$p$'s,$q$'s,$a$'s都是常数，由泰勒展开代入微分方程列出线性方程组解得。

一般会得到欠定方程组，不同的取值可以导出不同的方法：
- 一阶的欧拉法
- 二阶的Heun Method, Midpoint Method
- 更高阶的

RK导出二阶Henu法：
$$
   k_1 = f(t_i, y_i)\\
   k_2 = f(t_i + h, y_i + hk_1)\\
   y_{i+1} = y_i + \frac{h}{2}(k_1 + k_2)
$$

对于RK方法的框架来说，Henu相当于RK法$a_1 = a_2 = 1/2,\  p_1 = 1,\ q_{11} = 1$的特例

### 经典4阶RK方法

从RK Method框架导出的一个经典的4阶RK方法：
$$
   \begin{aligned}
      k_1 =& f(t_i, y_i)\\
      k_2 =& f(t_i + h/2, y_i + hk_1/2)\\
      k_3 =& f(t_i + h/2, y_i + hk_2/2)\\
      k_4 =& f(t_i + h,y_i + hk_3)\\
      y_{i+1}=& y_i + \frac{h}{6}(k_1 + 2k_2 + 2k_3 + k_4)
   \end{aligned}
$$

也即：

$a_1 = 1/6, a_2 = 1/3, a_3 = 1/3, a_4 = 1/6$

$p_1 = p_2 = 1/2, p_3 = 1$

$q_{11} = 1/2, q_{21} = 0, q_{22} = 1/2, q_{31} = q_{32} = 0, q_{33} = 1$

## System of ODE

很多时候，可以将高阶微分方程转写为一阶微分方程组。

通常，若$n$阶常微分方程组有如下通式：
$$
   y^{(n)} = f(t,y,y',y'', \cdots, y^{(n-1)})
$$

则采用如下换元：
$$
   \begin{cases}
      y_1 = y\\
      y_2 = y'\\
      y_3 = y''\\
      \vdots\\
      y_n = y^{(n-1)}
   \end{cases}
$$

可以得到一阶常微分方程组：
$$
   \begin{cases}
      y_1' = y_2\\
      y_2' = y_3\\
      \vdots\\
      y_{n-1}' = y_n\\
      y_n' = f(t,y_1,y_2,\cdots,y_n)
   \end{cases}
$$

然后套用解一阶常微分方程组.

如一个二阶常系数微分方程，可以写成两个一阶微分方程构成的方程组.

然后解微分方程组即可。

### Euler Method for system
$$
   \mathbf{y_{i+1}} = \mathbf{y_i} + \mathbf{f}(t_i, \mathbf{y_i})h
$$

其中，$\mathbf{y_i},\mathbf{y_{i+1}}$都是向量，$\mathbf{f}: \langle \mathbb{R},\mathbb{R}^{n}\rangle \to \mathbb{R}^{n}$

### Other methods for system

实际上，所有的RK方法都可以简单地将标量改写为向量来求解一阶的微分方程组（包括经典的4阶RK方法）。

## 隐式的一阶常微分方程求解
考虑一半形式的一阶常微分方程：
$$
   F(t,y,y') = 0
$$

看起来无法直接使用上述的方法，因为没有$y' = f(t,y)$中$f$的显式表达式，无法对给定的$t,y$求值。

但实际上，对给定的$t,y$，只要将原方程中的$t,y$看作常数，$y'$看作变量，然后原方程变为：
$$
   F(y') = 0
$$

这是关于$y'$的非线性方程，可以直接求解方程等价于得到给定自变量的$y'=f(t,y)$的函数值。（求解该方程可以使用牛顿迭代）

由此，使用牛顿法，我们可以在不求解$f(t,y)$的显式表达式的情况下求得其函数值，从而化归回到经典的一阶常微分求解方程。

## Boundary Value Problem (B.V.P)

### Definition & Theorm

考虑如下形式的边界值问题：
$$
   y'' = f(t,y,y'), y(a) = \alpha, y(b) = \beta
$$

我们有对该问题的李普希兹判据：
$$
   f_2(t,y,y') > 0\\
   |f_3(t,y,y')| < M
$$

线性边界值问题：
$$
   x'' = p(t)x' + q(t)x + r(t),x(a) = \alpha, x(b) = \beta
$$

代入李普希兹条件：

$$
   q(t) > 0\\
   |p(t)| < M
$$

我们只讨论线性边界值问题的求解：
- Linear Shooting
- Finite Difference

### Linear Shooting
利用线性边界值问题的特殊性，做如下构造：
$$
   x(t) = u(t) + Cv(t)
$$

然后**拆成两个IVP**：

$$
   u = p(t)u' + q(t)u + r(t), u(a) = \alpha, u'(a) = 0\\
   v = p(t)v' + q(t)v, v(a) = 0, v'(a) = 1
$$

求解这两个IVP，然后有原问题的解：
$$
   x(t) = u(t) + \frac{\beta-u(b)}{v(b)}v(t)
$$

其中$C$是根据定义解出来的