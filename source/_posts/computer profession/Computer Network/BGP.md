# BGP
## EGP
两种外部网关协议：
- EGP
- BGP

与内部网关的主要区别：
- 

## BGP概述
- BGP使用**可靠扩散**将AS内的网络的信息扩散出去
- BGP不要求AS连接的拓扑结构是一棵树，它有自己的处理回路的办法
- AS号类似IP地址，需要全球唯一，范围0~65535（部分是保留的私有AS号，不能在公网使用）

## BGP基本工作原理
### NLRI扩散
- AS中运行BGP协议的路由器称为BGP路由器，它充当BGP Speacker的身份；没有运行BGP则是内部路由器
- 在同一AS内的两个BGP路由器是iBGP相邻关系，而两个不同AS之间直连的BGP路由器称为eBGP相邻关系。
- BGP使用可靠扩散将AS内的网络的信息扩散出去
- BGP扩散的是NLRI(Network Layer Reachability Info.)
- 

BGP防止AS之间回路：由于NLRI中包含`AS_PATH`信息，记录一条NLRI的来路，因此在`AS_PATH`内的AS收到扩散NLRI，则不再扩散，丢弃。

BGP防止AS之内回路：我们规定：从iBGP收到的NLRI不能再扩散给iBGP。作为代价，一条从eBGP收到的NLRI必须单独发送给同一个AS内的所有iBGP，而不能只发送给自己的邻居iBGP。

内部路由器如何完成与AS之外的路由：
- 注入（不实用）：由BGP路由器为AS内的内部路由器注入所有的外部网络路由（太多了，内部路由器吃不消）
- 隧道：为所有的iBGP路由器之间配置“隧道”（类似直通链路）

### 建立BGP路由表
主要从NLRI包的信息构建BGP路由
（待完善）