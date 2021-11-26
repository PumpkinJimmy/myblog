# 分布式系统：同步
## 物理时钟同步
所谓时钟同步，主要是关注**计算机之间的始终保持一致**

物理时钟同步难以依靠单纯的CS技术处理，通常需要物理、通信、电子的技术来解决

两种常见的获取时间方式：
- 使用接收器接收UTC授时电波中接收
- **从时间服务器中获得时间**（这种方式更常用）
  
  容易看出一个问题，**网络通信本身有延迟，即便服务器的时间是绝对准确的，从服务器获得的时间也有偏差**

  工程上的解决方案是**使用往返的时间估计延迟，使用延迟最小的一次作准**

同步后发现始终不准怎么办：
1. 若本地时间比UTC慢，直接调快时钟即可
2. 若本地时间比UTC快，就有问题了：**一旦时钟拨回过去，一些已经产生的记录如何处置**。通常我们不会把时钟回拨。

## 逻辑时钟同步

相比物理上的“时间准确”，逻辑上的时钟同步的强调的是**时序正确**

逻辑时间不关心具体的时间，它通常讨论的是*计数器的计数*

### Happen-before

Happen-before是一种事件之间约束，若一个事件在另一个事件之前发生，则它们之间存在Happen-before。

自然中存在基本的、可以不加证明的Happen-before关系，比如说“发送信息发生在接收信息之前”


### 基本逻辑时钟算法
考虑存在一个计数器，初始为0
1. 每次发生一个新事件，计数器+1
2. 每次发送一个消息时，消息使用计数器的计数作为时间戳
3. 每次接收到一个消息时，系统检查消息的时间戳和计数器的时间。**当前计数器的值设置为两者中较大的一个**。

这个简单方法的核心思想就是**发送消息在接收消息之前**。也就是说，如果消息中的时间戳比本地时钟还要领先，则必须调快本地时钟。

### 实例：副本数据库更新
考虑DB1和DB2是复制数据库，P1和P2是系统的两个操作员：
- P1为账户存入100元（记作T1），P2为账户结算1%利息（记作T2）
- 由于是复制数据库，DB1和DB2必须都在数据库上执行T1和T2
- 但若DB1和DB2在执行T1和T2的顺序上不一样，就会导致账户余额在DB1和DB2上不一致
- 即便在DB访问中全部使用了正确的数据锁，也有可能产生不一致

可以发现：
- 两个事务是并行的
- 在特定的DB主机而言，正确的程序实现中可能会对账户数据上锁，因此本机上的一致性不成问题。不一致性并不因为操作的汇编指令的交叉执行。
- 真正的问题出在T1和T2同时在DB1和DB2上执行，修改同一块数据，**但先执行T1还是先执行T2是不确定的，而且T1和T2的执行顺序会影响最终的结果**
- 注意区别OS中的基本竞态问题：**单机中运行的程序里，一旦通过互斥实现临界操作线性化问题就解决了**；但在复制数据库中，**每一个数据库有如平行的世界线，是独立的状态机，我们不但要保证临界操作线性化，还要保证各个副本上的临界操作具有相同的顺序**

在分布式系统上我们需要比信号量更强的互斥锁。

### 全局多播

全局多播可以实现**各副本上进程进入临界区的顺序相同**，更具体地，**保证临界事件的交付(Deliver)顺序相同**


## 互斥算法
- Centralized 使用一个Master节点控制所有的临界区访问
- Distributed 一个节点需要访问临界区，需要取得所有的节点的同意
- Token ring 节点在逻辑上围成环，环中一个令牌（Token）在轮流传递。得到令牌的节点才能访问临界区
- Decentralized （待完善）

观察发现上面这些互斥算法同样出现在计网的共享信道争用的问题中：
- Centralized 集中式的信道管理
- Take Turn 对应令牌环网
- Random Access 对应CSMA/CD以及CSMA/CA

本质上是因为**共享信道就是分布式系统中的一种共享资源**

上面这些互斥算法都有针对不同场景的混合、改进。

## 选举算法

以下考虑一种这样的模型：
- 系统中所有进程的ID对所有的进程都是已知的
- **系统中ID最大的有效节点自动称为Master**

### Bully算法

则采取如下方式*选举*出系统的中心节点：
1. 当进程$P_k$发现Master失效，则可以发起选举，**向$P_{k+1}, P_{k+2}, \cdots$发送`ELECTION`消息**
2. 若没有得到任何回复，则$P_k$成为Master

这个算法的思想贴合其名称：*霸凌(Bully)*

### Ring算法

考虑系统节点构成环，当发现Master节点失效的时候，**节点沿着环发送选举消息**，消息转一圈之后就可以重新选举出Master了

### 生成树算法
1. 按照生成树方法发送消息
2. 向生成树的父亲发送当前了解到的最大节点
3. 最终在树根知道了系统中的最大节点