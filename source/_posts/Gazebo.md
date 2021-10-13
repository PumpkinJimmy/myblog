---
title: Gazebo
categories:
  - - 机器人
    - 仿真
tags:
  - Gazebo
  - 机器人
  - 仿真
abbrlink: c795f955
date: 2021-09-07 10:35:42
---
# Gazebo笔记
## 仿真机器人描述
为了正确启动仿真，需要提供一些机器人的参数。

对仿真机器人描述通常包含：
- 部件(Link)
- 连接(Joint)

而部件是包含：
- 外观(Visual)
- 惯性参数(Ineria)
- 碰撞属性(Collide)
- 发光属性(Light)

的仿真机器人组件对象。

现存多种描述机器人的文档格式，包括URDF, Xacro, SDF