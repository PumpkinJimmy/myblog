---
title: Outline
tags:
  - 专业课
abbrlink: 88c26b79
date: 2021-06-15 11:38:32
updated: 2021-06-15 11:38:32
---
# OS Outline
- Intro
  - OS的核心功能
    - 用户-硬件接口
    - 资源管理调度
    - 可拓展平台
  - IO基本策略
    - 基于轮询
    - 基于中断
    - DMA (Direct Memory Access)
  - 中断
    - 硬件中断（可屏蔽）
    - 软中断（不可屏蔽）
    - 异常中断（不可屏蔽）
  - 对称多处理器

- 进程
  - 进程定义：运行中的程序在OS中的实体
  - 进程映像 = 代码段 + 数据段 + 栈段 + PCB
  - PCB
    - pid
    - 进程状态
    - 优先级
    - 处理器上下文（寄存器）
    - 内存相关指针
    - IO信息
    - 统计信息
  - 二状态进程模型：运行 & 就绪
  - 五状态模型
    - 运行态
    - 就绪态
    - 阻塞态
    - 起始态
    - 结束态
  - 六状态进程模型
    - 挂起态
  - 进程控制原语：OS提供的原子操作

- 线程
  - 进程 vs. 线程
    - 进程是资源分配的最小单位
    - 线程是处理器调度的最小单位
  - 用户级线程 vs. 内核级线程
  - 对称多处理（SMP）与多线程架构

- 并发性：互斥与同步
  - 竞态问题
  - 临界区
  - 互斥的六个要求
    - 强制排他：不可同时进入临界区
    - 有限等待：不会饿死、不会死锁
    - 满足异步：与相对并发性能无关
    - 充分并发：不影响非临界区代码并发
    - 让权等待：进程无法进入临界区时立刻让出处理器，减少忙等待
    - *空闲让进：没有代码试图进入/已经进入临界区时，一个进入临界区的尝试能立即成功
  - 互斥-软件实现
    - Dekker算法
    - Peterson算法
    - 软件实现的问题：**性能一般，忙等待**
  - 互斥-硬件实现
    - 单处理器互斥：关中断
      - 限制了处理器交替执行的能力
      - 不适用于多处理器架构
    - TS/XCHG自旋锁
      - 忙等待（自旋锁）
      - 性能高
      - 适用多处理器（只要共享主存）
      - 具体实现
  - 信号量
    - 信号量的操作
      - 初始化（初始值很重要）
      - P操作
      - V操作
    - 信号量实现
      - 自旋锁维护共享整数变量
      - 维护信号量阻塞队列
    - 信号量应用
      - 互斥锁
      - 生产者-消费者问题
      - 读者-写者问题
  - 管程
  - *条件变量

- 死锁
  - 死锁发生的条件：
    - 互斥
    - 占有等待
    - 资源不可抢占
    - 循环等待
  - 死锁预防
    - 破坏互斥：不可能
    - 破坏占有等待：初始申请所有资源，不允许资源动态申请
    - 允许资源抢占
    - 破坏循环等待：按需分配
  - 死锁避免：银行家算法
    - 进程启动拒绝
    - 资源分配拒绝
  - 死锁检测：
    - 杀死死锁进程
    - 回滚
  - 哲学家就餐问题
    - 朴素实现：死锁
    - 基于管程 & 条件变量

- 内存管理
  - 覆盖与交换
  - 分区管理
    - 固定分区：内碎片
    - 动态分区：外碎片
    - 分区放置策略
  - 分页管理
    - 线性地址<->物理地址
    - 优点
      - 一维地址
      - 消除碎片
      - 用户地址空间转换
    - 缺点
      - 难以共享
    - 页表
      - 多级页表：页目录
      - 反向页表
      - TLB页表高速缓冲
  - 分段管理
    - 段号:段内偏移<->线性地址
    - 优点：
      - 契合程序逻辑结构
    - 缺点：
      - 产生碎片
  
- 虚拟存储器
  - 虚拟分页
  - 分页管理策略
    - 调入策略
    - 替换策略
    - 驻留集管理
    - 替换范围
    - 清除策略
  - *虚拟分段

- 调度
  - 调度的类别
    - 长程调度：作业调度
    - 中程调度：虚拟存储器 & 挂起
    - 短程调度：处理器分派
  - 短程调度类别：
    - 抢占式
    - 非抢占式
  - 非抢占式短程调度策略：
    - 先来先服务（FCFS）
    - 最短进程优先（SPN）
    - 最高响应比优先（HRRN）
  - 抢占式短程调度策略：
    - 时间片轮转（RR）
    - 虚拟时间片轮转（VRR）
    - 最短剩余优先
  - 基于优先级的短程调度策略：
    - 最高优先级优先（HPF）
    - 可抢占式优先级（Peemptive HDF）
    - 多级队列反馈（MF/FB）
  - 公平共享调度

- IO
  - IO设计的目标
    - 效率
    - 通用性
  - IO类型
    - 程序控制IO
    - 中断驱动IO
    - 基于DMA的IO
    - *更高级IO技术
  - IO系统架构（待完善）
  - SPOOL假脱机技术
    - 高速虚拟IO操作
    - 实现对独占设备的共享
