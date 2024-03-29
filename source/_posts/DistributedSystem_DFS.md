---
title: 分布式文件系统
abbrlink: b664abf
date: 2021-12-17 07:03:51
---
# 分布式文件系统
## Why RPC
- 实现语言无关、OS无关、FS无关和网络传输协议无关
- 问题：RPC的穿透能力较差
## NFS
- 远程的文件操作系统，但不是典型的分布式
- 基础的架构：FS操作的执行全部发生在服务器上。客户端向服务器发送FS请求，返回操作结果作为返回
- 有无状态
  - 无状态：不记录客户端信息
    - Crash之后重启不需要恢复
    - 性能较差
  - 有状态：记录客户端信息
    - 性能好
    - Crash重启需要恢复和同步
## GFS/HDFS
- 基于Master/Chunk，或者说Namenode/Datanode
- **Master只提供文件索引服务，FS操作只做Log，具体读写交给Chunk**
- Master只负责文件索引和日志，**保证了Master不会称为瓶颈，且少量Master就可以管理大量Data Node**

## Ivy
- 基于DHT的P2P文件系统