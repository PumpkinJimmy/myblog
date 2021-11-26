---
abbrlink: '0'
date: 2021-09-24 11:12:27
---
# 点云滤波
- 直通滤波：直接丢弃给定坐标范围之外的点
- 离群点去除：以某些标准判定一个点是否是离群点，并移除
  - 依据半径：给定一个半径
  - 统计：在邻域内做正态建模，然后移除不符合模型的点
- 双边滤波（待完善）
- 体素降采样