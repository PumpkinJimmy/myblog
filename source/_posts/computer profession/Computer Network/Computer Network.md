---
title: Computer Network
tags:
  - 专业课
abbrlink: acc2fc2f
date: 2021-06-02 10:46:51
updated: 2021-06-02 10:46:51
---
# Computer Network
## Introduction
### 网络协议层次结构
OSI七层参考模型：
- 应用层
- 表示层
- 会话层
- 运输层
- 网络层
- 数据链路层
- 物理层

常见教学五层结构：
- 应用层
- 运输层
- 网络层
- 数据链路层
- 物理层

实际Internet四层结构：
- 应用层
- 运输层
- 网络层
- 物理网络层

应用层：
- 为*专门的应用*提供服务
- 传输单位为Message（报文、消息）
- 协议非常多，比如：
  - HTTP: Web
  - SSL/TLS: Security
  - SMTP/POP3: Email
  - DNS
  - FTP: File
  - DHCP: Configuration
- 由于Unix风格和历史原因，应用层协议偏好*基于文本*的形式

表示层：
- 数据的表示、压缩、转换、加密
- 对标HTTP *Accept-Encoding*与SSL/TLS

会话层
- 提供会话功能（比如IP电话、视频）
- 对标RTMP、RTP等流媒体传输协议

传输层（运输层）：
- 提供*进程与进程*之间的通信
- 传输单位为Segment（报文段）
- TCP、UDP
- 这一层的TCP可以提供*有连接*、*有确认*的*可靠传输服务*

网络层（网际层）：
- 提供*主机与主机*之间在*互连网络*中的通信
- 传输单位为Datagram（数据报）
- 最重要的功能其实是**路由选择**
- IP协议的提供的服务是*无连接*的*不可靠服务（尽最大努力交付服务）*
- 协议：
  - 代表性协议是*IP*
  - 路由选择协议
    - RIP
    - OSPF
    - BGP
  - ICMP
- IP协议实际上是因特网协议族的最底层，**物理网络层不属于因特网协议族**。这是因为**IP协议连接的物理网络的各个子网可以是异构的**，或者说互连网络连接的各个子网可以采用不同的链路层协议和物理层协议

数据链路层：
- 提供主机与主机之间在*直连网络*中的通信
- 传输单位为Frame（帧）
- 负责数据的校验
- 协议：
  - PPP（点到点）
  - Ethernet（多路访问网络）
  - ARP（基于MAC地址的链路层通信）

### Concepts
- SAN, LAN, MAN, WAN
  
  SAN: System Area Network
  
- Internet
  
  Internet = Host + Link + Router

- PDU, SDU, PCI, SAP
  - PDU: Protocol Data Unit，也即包（Packet）
  - SDU: Service Data Unit; PCI: Protocol Control Information，一般作为header
  - PDU = PCI + SDU
  - SAP: Service Access Point

- 时延：
  - 处理时延：与路由器处理性能有关
  - 排队时延：与拥塞有关
  - 传输时延：与通信链路带宽有关
  - 传播时延：
    - 与通信介值、通信距离有关
    - 注意**光纤的传播时延大于铜线**

- 因特网：全球最大的互联网
  - 因特网=边缘部分+路由器+通信链路
  - 因特网=边缘部分+接入网+主干网

- 带宽 vs. 吞吐量
  - 带宽：通信链路的数据率的理论上限
  - 吞吐量：通信链路实际的数据率
  - 通常吞吐量达不到是跑不满带宽的。原因很多，包括：
    - 通信环境差，信号失真严重，数据在传输中出现差错
    - **协议控制信息/ARQ重传的额外开销**。注意到**链路为主机提供了数据通信的通道，但通道上传输的不一定是有效载荷，非有效载荷的部分都不记作有效传输的数据，最终都会成为降低吞吐量的部分**

## 信道复用

- Multiplex
  - FDM: Frequency Division Multiplex
  - TDM/WDM: Time/Wave Division Multiplex
  - CDM: Code Division Multiplex (3G: CDMA)
  - Statistical Multiplex

- Switching
  - Circuit Switching: FDM,TDM,CDM, Telephone 
  - **Packet Switching**: Statistical Multiplex, Computer Network

