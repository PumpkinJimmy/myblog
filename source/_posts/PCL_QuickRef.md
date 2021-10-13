# PCL QuickRef.
```cpp
// 基本数据类型
PointCloud
PointXYZ
PointIndices //< 点云索引子集，相当于Mask

// --- IO ---
loadPCDFile

// --- 滤波 ---
VoxelGrid //< 体素化下采样
Passthrough
StatisticalOutlierRemoval

// --- 分割 ---
RegionGrowing // 区域生长分割
ExtractIndices // 根据给定PointIndices提取点云子集

// --- RANSAC ---

// --- 特征 ---
NormalEstimation // 法向量估计

// --- 配准 ---
IterativeClosestPoint // < ICP

// --- 重建 ---
MovingLeastSquare // < 最小二乘法
ConcaveHull // < 提取多边形
GreedyProjectionTriangulation // < 贪婪三角形网格提取

// --- 可视化 ---
CloudViewer
PCLVisualizer
    .spinOnce(n) // 主循环n次
    .spin() // 主循环
```