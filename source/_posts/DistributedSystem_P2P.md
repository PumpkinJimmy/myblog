---
abbrlink: '0'
date: 2021-09-24 14:31:26
title: P2P

---
# P2P
## 穿透问题

在P2P系统中最显著的问题是*防火墙的穿透问题*。

典型的PC因特网访问方案：
- PC位于某个私有网络中
- 私有网络中存在一个**NAT网关**
- PC通过NAT网关连接到因特网上
- PC默认只能连接持有公网IP的服务器
- 服务器**通过NAT网关与PC间接通信**，PC的对服务器**不可见**

在典型的C-S模式中，Server是持有公网IP的，因此NAT网关不成问题。

但在P2P中，**不存在典型的服务器，通信双方的主机可能都在某个私网内，通过NAT网关连接因特网**

解决方案是：
- 网关之间交换控制信息
- 主机之间穿透防火墙直接交换数据信息