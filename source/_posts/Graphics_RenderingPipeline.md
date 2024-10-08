---
title: Graphics_RenderingPipeline
abbrlink: 35bde018
date: 2024-08-18 13:06:04
tags:
categories:
---
# 图形渲染管线
## GPU硬件构成笔记
- 术语
  - GPR: General Purpose Register，类似CPU的Register集合
    - GPR file表示一组计算单元对应的寄存器上下文的集合
  - Host/Device
    - 来自NV的说法，Host表示CPU以及主存构成的宿主计算系统，Device表示GPU和（可能存在的）显存构成的额外计算系统
- Shader Core 
  - 一个指令buffer，n个计算单元（ALU/FPU），n组GPR
  - SIMT，同时对一个指令流和n组数据执行n个thread
- Workgroup：
  - 一个能够在Shader Core上运行的线程组
  - nv上就是warp，amd上是wavefront，mali上叫做quad
  - 由于每个GPR file能够表征一个thread的当前运行状态，所以我们用一个GPR file表示一个thread，一个workgroup的实际内容可能就是n个GPRfile的数据（指令buffer可以认为是固定无法切换的）
- Latency Hiding
  - 通常来说，Workgroup的数目大于Shader Core的数目，一个Shader Core拥有多个Workgroup，*Core中同时执行的Workgroup只有一个*
  - 当一个Workgroup执行到Stall的时候（比如Sampling），立刻切换到其他Workgroup执行
   - 类似单核多线程的并发 
    - 目的是隐藏这个Stall的延迟（该延迟可达到100~1000个Cycle）
    - 代价是会延长某个WorkGroup的总完成时间（增大时延）
    - 好处是通过充分利用Shader Core提高了Throughput
- Unified Shader Core
  - 一个通用Shader Core在调度器的控制下装入一个Shader程序，可以是VS/GS/FS，然后调度多个Workgroup执行Vertex Stage或者Fragment

## 移动端GPU
### 市场上的常见架构
- Mali
- Adreno
  - 源自AMD，常见于高通
- PowerVR
  - 苹果是类似的

### TBR
- *移动端GPU没有自己的显存*
  - PC端不但由自己的显存，而且有On-board memory（Global Memory/Local Memory）和on-chip memroy（Register/Shared Memory）
  - 移动端的显存只能占用主存的一块区域
  - 这意味着，移动端的GPU和显存通信相当于PC端的Device和Host通信，这样的通信占用带宽巨大且发热严重
- 考虑到在FS中会频繁的读写Frame Buffer
  - 注意，由于有限的寄存器以及大量的Frame Buffer像素，多次读取主存上的Frame Buffer将不可避免
  - 因此使用*Tile-based Rendering*
- TBR
  - 在VS阶段一切如常
  - 在FS阶段**每次将一个Tile一次性载入GPU上的Local Tile Memory中**，然后GPU中的FS就可以读写Local Tile Memory而不是主存上的Frame Buffer来执行FS阶段的操作。执行完之后再将Tile的数据写回到Frame Buffer中
  - Host-Device带宽的节省：从取决于FS的n次读写降低到固定的1读1写
  - 好处是节省了带宽，降低了发热；代价是GPU每次只能渲染一个Tile，无法充分并行