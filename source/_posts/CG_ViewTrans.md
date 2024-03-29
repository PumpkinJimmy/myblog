---
abbrlink: '0'
date: 2021-09-17 10:03:56
title: 图形学：视图变换

---
# 图形学：视图变换
视图变换主要分为如下几个部分：
- Model 模型变换
- View 视点变换
- Projection 投影变换

## 模型变换

将模型从模型坐标系变换到世界坐标系

其实就是把模型放在特定位置

## 摄像机变换

视点变换，也称“摄像机变换”，根据摄像机进行变换

说是说变换，但导出的思维方法最好是“不同坐标系下的表示”，或者说“基变换”。

但是，通常摄像机坐标系在世界坐标系上的表示是容易描述的，但我们要的是“世界坐标系中的点在摄像机坐标系上的坐标表示”，变换矩阵等价于“世界坐标系在摄像机坐标系下的表示”。

因此，摄像机变换等价于*摄像机坐标系在世界坐标系下的变换矩阵的逆矩阵*。

这里存在一个线性代数的技巧：若摄像机坐标系是一组*单位正交基*，则*逆矩阵即转置矩阵*

## Projection

投影变换，将3D世界中的存在在视野内的点根据给定的透视方案投射到二维平面上。

这里其实可以分成两部分：
- 将视野区域（称为*视锥体*）映射到$[-1.0, 1.0]^2$范围的设备标准坐标系上。这部分可以称为“透视变换”
- 将$[-1.0, 1.0]^2$变换到具体设备屏幕上。这部分可以称为“视口变换”
- 注意，OpenGL中顶点着色器只做到透视变换，输出的是$[-1.0, 1.0]^2$的设备标准坐标系中的点，视口变换是后面的事。

最常用的是
- 正交投影 orthographic projection
- 透视投影 perspective projection

### 视锥体

其实就是“舞台”。**只有落在视锥体内部的对象最终能投影到屏幕上。