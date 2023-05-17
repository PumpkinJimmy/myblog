---
date: 2021-12-03 11:23:39.899593+08:00
title: 曲面细分

---
# 曲面细分
## 三角网格细分
- Loop
  - 基于*Approximation*
  - C2连续
- Butterfly
  - 基于*Interpolation*
  - 保形性好
  - C1连续

## Catmull-Clark Subdivison（CC细分）
- 可以处理任意多边形网格
- 在Regular点达到C2，非Regular点达到C1