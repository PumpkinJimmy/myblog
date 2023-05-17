---
title: 基本阴影生成
abbrlink: 7eea30f0
date: 2021-10-15 10:04:59
---

# 基本阴影生成

考虑光照遮挡生成的

## 基本概念
- Receiver
- 遮挡物
- 本影
- 半影（penumbra）

## 基本类别
- Attached Shadows 附着阴影
  
  由于背向光源而导致的*附着于模型表面*

- Cast Shadows 投射阴影
  
  由于*光照遮挡*导致的*投射于Receiver*的阴影

- Self Shadows
  

## Hard Shadow vs. Soft Shadow
- Hard Shadow
  
  通常由理想点光源生成。阴影存在硬边界，不自然

- Soft Shadow
  
  引入面光源和半影区使得阴影更自然

## Planar Shadow

利用相似，将顶点投影到给定平面上。

然后在着色时**不计算漫反射和镜面反射，只算环境光**

## Curved Surface Shadow

将模型渲染为全黑得到一个影子，然后当作纹理贴到Receiver上。

缺点是无法渲染自遮挡产生的阴影。

## Shadow Map

基于z-buffer的阴影生成，用于**快速判断顶点是否被遮挡**，可以支持自遮挡。效果可以，且性能极佳。

两步走生成阴影：
- **将摄像机放在点光源的位置，渲染计算z-buffer，生成Shadow Map**
- 将摄像机放回原位，利用Shadow Map确定每个顶点是否在阴影内

注意：**无论摄像机如何变化，只要光源和遮挡物的相对关系不变，Shadow Map就不变**

优点：
- Shadow Map在静态场景渲染中只需要计算一次，性能高。

缺点：
- Hard Shadow （点光源）
- 重采样导致的各种问题（Shadow Map的分辨率是有限的）
- 关于浮点运算的一些问题

## Shadow Volume
可用于动态场景的阴影生成方法。

对物体计算**Shadow Volume**（阴影体），然后给出摄像机，利用*阴影体前向面/后向面*来确定顶点是否存在于阴影体内，最终确定点是否被遮挡。

优点：
- 可以用于动态场景
- 不存在分辨率问题
- 精细度高

缺点：
- 计算开销大