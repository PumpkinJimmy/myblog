---
date: 2021-09-30 16:57:13
---
# 图像频率域处理
## 图像DFT
DFT
$$
    F(u,v) = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} f(x,y) e^{-j 2\pi (ux/M + vy/N)}
$$

IDFT
$$
    f(x,y) = \frac{1}{MN} \sum_{u=0}^{M-1} \sum_{v=0}^{N-1} F(u,v) e^{j2\pi (ux/M+vy/N)}
$$

## 性质
- 线性
- 空间平移性质
- 移频性质
- 周期性
- *旋转性质