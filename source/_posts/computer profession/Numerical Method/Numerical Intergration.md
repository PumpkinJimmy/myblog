# Numerical Intergration（数值积分）

## Trapezoid's Rule（梯形法）

使用以积分区域端点为上下底的梯形计算积分。

误差大，计算简单，形式实际上就是梯形面积公式。

### 公式

let $h = b - a$
$$
    \int_{a}^{b} f(x) \mathrm{d}x \approx \frac{h}{2}[f(b) + f(a)]
$$

### 误差

$O(h^2)$近似.在$h>1$时误差非常大

## Simpson's Rule（辛普森法）

**辛普森研究发现，梯形法实际上是对两个点的拉格朗日插值多项式积分的结果**

因此，Simpson's Rule就是假设取样点都是等间隔的，然后对拉格朗日插值多项式做积分推导出来的。

### Simpson 1/3

Assume that the points are equally spaced.

$$
    \int_{a}^{b} f(x)\mathrm{d}x \approx \frac{h}{3}[f(x_0) + 4f(x-1) + f(x_2)]
$$



### Simpson 3/8

Assume that the points are equally spaced.

$$
    \int_{a}^{b} f(x)\mathrm{d}x \approx \frac{3h}{8}[f(x_0) + 3f(x-1) + 3f(x_2) + f(x_3)]
$$

## Boole's Rule

Assume that the points are equally spaced.

$$
    \int_{a}^{b} f(x)\mathrm{d}x \approx \frac{2h}{45}[f(x_0) + 3f(x-1) + 3f(x_2) + f(x_3)]
$$

## Newton-Cotes Open Formula

（待完善）

## Better Numerical Integration

### Composition Integration（复合积分）

基本数值积分公式巨大误差的来源是积分区间过大。

可以将积分区间划分成宽度足够小（小于1）的积分区间分段积分。这种方法就是Composition Intergration。每个区间上可以使用梯形法，也可以是Simpson。当然Compostion Integration需要许多个点。

还有一个好处，Composition Integration容易用于不等间隔情况。

### 

（待完善）