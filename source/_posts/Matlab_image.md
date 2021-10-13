---
abbrlink: '0'
date: 2021-09-10 22:18:57
---
# Matlab 图像处理
```matlab
size % 图像大小
class % 图像类型
zeros((m,n), "uint8") % 空图像
imread % 导入图像
rgb2gray % 转为灰度图
imhist % 计算并显示直方图
im2uint8 % 将[0,1]范围的double类型图像转为[0,255]的uint8图像
conv2 % 二维卷积，可以用于做线性空间滤波

sum % 求和，默认是对一条轴求和
sum(my_arr, 'all') % 求总和
img == 0 % 布尔矩阵
img(:) % flatten
img([1 2 3]) % 花式索引
[arr1 arr2] % 拼接：按列
[arr2:arr2] % 拼接：按行

title % 标题（注意先画图再写标题）
subplot(m,n,idx) % 子图
xlabel, ylabel % 横纵轴标签
xlim, ylim % 轴范围
xticks, ytikcs % 轴刻度
```