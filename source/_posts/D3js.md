---
title: D3.js简记
abbrlink: 2d51d24b
date: 2021-10-20 16:58:39
---
# D3.js简记

D3.js根本就不是传统意义上的一个绘图工具箱。

D3.js实际上是一个面向数据的、基于数据绑定的前端绘图框架。

因此它的核心组件根本不是绘图函数而是**DOM选择和数据绑定**。

除了核心组件，D3提供了便于构建图像的一些工具集。比如色彩插值、比例尺映射、动效支持、SVG的`<path>`模板、甚至是各种绘图布局方案和地理绘图支持。**D3提供的几乎所有组件都不含直接绘图功能，最终产生视觉效果完全靠CSS3和SVG**。

D3的设计思维是：**通过提供核心组件允许图像与数据动态绑定，并且提供丰富的工具集用于指明数据如何渲染到CSS3/SVG标签中，实现面向数据的灵活可视化功能**

D3甚至没有提供一个绘制曲线图的基本函数，但高度灵活的框架允许几乎任何类型图像的绘制，而且支持动效和交互。

D3的缺陷：
- 一开始比较难上手，因为不能调一个函数直接画图
- 虽然说面向数据，但是针对数据的动态绑定更新非常不直观，尤其是`data()/enter()/exit()`选择器非常反人类思维的设计
- 可能存在性能。对于游戏类及其强调实时的图形以及交互性能的场合，WebGL/Canvas标准更合适，对应类似Three.js, Canvas Engine的库

## Quick Ref
```javascript
d3.select
d3.selectAll
d3.create

selection.append
selection.create

selection.data
selection.enter
selection.exit
selection.remove

selection.attr
selection.style
selection.classed

selection.transition
selection.tween

selection.each
selection.call
selection.nodes

d3.extent

d3.line
line.curve
line.defined

d3.arc
d3.pie
d3.area

d3.scale
scale.domain
scale.range
scale.ticks

d3.axisBottom/d3.axisRight/d3.AxisTop/d3.axisLeft
axis.scale
axis.ticks

selection.transition
transition.ease
transition.
```

## D3数据更新
这是最强大但最难理解的部分。

回忆Vue数据绑定模型：
- 简单的*双向绑定模型*
- 初始化传入一个Object，然后就把一些数据绑定到了Vue实例上了
- 这个Object是数据绑定的主体：修改这个Object的属性会同步到Vue实例上；Vue实例中对Data Object属性的修改也会同步到外部的引用中
- 缺陷是不能简单地增加和删除属性

D3.js的数据绑定
- 必须在selection上调用data来做数据绑定
- 没有采用将属性添加为property来实现响应式自动追踪的效果
- 绑定的数据不一定是Object，也可以是数组（倒不如说主要是数组）
- 可以实现数据增加和删除（依靠`enter()`和`exit()`）
- 依靠的思维方式是**提供数据的键来匹配**

## D3 Scale
非常便利的工具集，满足日常各种尺度映射问题。

Scale的作用，一言以蔽之，就是**方便地构造从一个集合到另一个集合的映射函数**

以下一些功能容易用Scale达成：
- 对数比例尺
- 离散集合到连续，用于从标签到屏幕空间坐标
- 单纯的线性映射，用于从数据到屏幕空间坐标
- 连续数值离散化的映射


```javascript
d3.scaleLinear // 连续区间线性映射到连续区间
d3.scaleBand // 离散集合映射到连续区间
d3.scaleQuantize // 连续数值离散化到标签
```

## Misc
- `selection.node()`返回选择元素的DOM Element对象
  
  这样做的主要动机是`d3.selection`不可能代理所有的Element接口，有些场合、有些调用必须用原始的Element
