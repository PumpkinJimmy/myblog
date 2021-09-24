---
title: OpenGL
date: 2021-09-03 10:50:28
updated: 2021-09-03 10:50:28
tags:
- 专业课
---
# OpenGL笔记
## GLFW和GLAD
   
   *GLFW*是对OS的GUI接口的封装。跨平台创建窗口、检验实践。

   *GLAD*省了调用的加载。

## VAO, VBO, IBO
- *VAO* 顶点数组对象
- *VBO* 顶点缓冲对象
- *IBO* 索引缓冲对象

## 基本渲染管线
1. *Vertex Shader* 顶点着色
2. *Primitive Assembly* 图元装配
3. *Geometry Shader* 几何着色
4. *Rasterization* 光栅化
5. *Fragment shader* 片元着色
6. *Blending* 混合

## 常用API
```cpp
glGenBuffers(); // 创建缓冲
glBindBuffer(); // 绑定缓冲类型
glBufferData(); // 数据传送到缓冲
glVertexAttribPointer(); // 配置着色器如何解释缓冲区中的数据输入
glEnableVertexAttribArray();
glBindVertexArray();
```

## 着色器
注意区分*着色器(Shader)*和*着色器程序(Shader Program)*

一个着色器程序是由多个着色器前后级联构成的一个完成渲染模块，也就是*可编程渲染管线*。

每个着色器虽然是独立的程序，但也是着色器程序的一部分。

着色器使用`in/out`关键字来指明着色器的输入、输出变量。

### 顶点着色器
顶点着色器是基本渲染管线必须的一部分。它的输入不一定是标准坐标，但是输出必须是标准化设备坐标。

在顶点着色器之后的所有坐标都应该使用标准化设备坐标。

### 片元着色器
输出一个颜色，表明片元的颜色

### Uniform
使用`uniform`关键字定义可以供整个着色器程序中任何一个着色器使用的“全局”变量。

由于是全局的，可以在任何一个着色器中声明Uniform。

使用`glGetUniformLocation`在主程序中获得Uniform变量的引用，以便修改。

注意，**要先启动着色器程序，才使用GetUniform!**


## 理解VBO，EBO，VAO
- 顶点属性
  - 其实就是一个顶点可以具有很多属性
  - 除了最基本的坐标属性（位置），还可以有：颜色、透明度，甚至是法线的坐标表示
- VBO
  - 一块拿来放顶点属性的缓冲区
  - 可以申请很多块VBO
  - 使用`glBufferData`把数据从CPU拷到VBO中
  - 但每次用于渲染的顶点只能是指定的某一块VBO缓冲
  - 使用`glBindBuffer(GL_ARRAY_BUFFER, your_vbo_buf)`来指明渲染哪一块VBO中存放的顶点
  - 为了让着色器可以正确解释VBO中的字节，必须手动指明顶点属性的存储模式（其实相当于struct指明内存分布）
  - 使用`glVertexAttribPointer`指明顶点属性的存储模式
  - 需要使用`glEnableVertexAttribArray`来启用属性
  - **数据只需要传输一次，以后就不必再重复传输了**
- VAO
  - 所有的VBO配置工作只要做一遍，之后直接从VAO中载入配置然后渲染
  - 可以理解为一个“配置录制工具”，将配置录制到VAO中，然后每次再载入配置即可
  - 存储的配置包括：
    - `glBind`绑定的是哪一块VBO
    - `glVertexAttribPointer`配置的顶点属性模式，启用的节点
    - 绑定的EBO
  - 其实OpenGL是一个状态机，如果只使用一个VBO，那其实用不上VAO；但考虑到可能使用多个VBO，它们各自对应不同的顶点属性模式，每次更改绑定的VBO，都要重新配置属性，这就很麻烦，因此为每个VBO配套使用一个VAO是有好处的
- EBO：减少重复顶点
  - 在VBO中只存储不重复的顶点和它们的属性
  - 实际渲染的时候使用EBO中的索引值在VBO中取点

## OpenGL的顶点和PCL的点云的区别
- OpenGL的Vertex和PCL的Point都是一个具有三维坐标以及其他属性的抽象的“点”
- 但OpenGL的顶点并不是孤立的，OpenGL需要*组装图元*，也即某些点组装成三角形。

## 纹理
所谓*纹理(Texture)*是贴图的意思。

纹理可以是2D也可以是3D甚至有别的，这里主要讨论2D纹理。

为了将纹理贴在正确的位置，需要为顶点指定*纹理坐标*。

纹理坐标是指，将纹理归一化放在纹理坐标系的$(0,0)$到$(1,1)$矩形区域上，然后顶点相当于映射到纹理坐标系的某个位置，然后根据坐标映射关系OpenGL将纹理贴到正确的位置去。

### 使用纹理
- `glGenTextures`申请一个纹理对象
- `glActivateTexture`激活一个纹理单元（供片段着色器渲染使用）
- `glBindTexture`将某个纹理对象指定为当前纹理
- `glTexPeremeteri`配置纹理属性，包括延展的策略和放大插值的策略
- `glTexImage2D`将一张图片的数据传输到纹理对象的缓冲区

在片段着色器中使用sampler2D类型的Uniform输入变量来输入纹理单元，使用`texture`函数来使用纹理。

## 变换
### 齐次坐标
可以发现，顶点着色器中输出的标准化坐标存在第四个恒为1的分量，这个分量称为*齐次坐标*。这是因为线性变换不能表达“平移”操作，为了方便增加了这个坐标。

### 万向锁
使用关于$x,y,z$轴的偏角（称为*欧拉角*），我们可以给出任意一个3D空间中的旋转。不仅如此，由于角是任意角，我们还可以通过插值得到旋转的整个轨迹。

但是，存在一些角，具有不唯一的旋转轨迹，从而无法使用一组欧拉角来表征唯一的运动轨迹。

### GLM
为OpenGL定制的矩阵运算库，可以方便地生成各种变换矩阵。

**陷阱：glm::mat4必须显式初始化**:
`glm::mat4 trans = glm::mat4(1.0f);`


### 摄像机变换与Lookat

所谓摄像机变换，其实是把世界坐标系上的点变换到摄像机坐标系。

利用基变换思维可以写出变换矩阵。

`glm::lookat`可以便捷地生成这样地变换矩阵

```cpp
trans = glm::lookat(cameraPos, lookatPoint, cameraUp);
```