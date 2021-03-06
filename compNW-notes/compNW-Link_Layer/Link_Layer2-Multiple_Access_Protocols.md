# Multiple Access Links and Protocols

## Introduction

+ 有两种链路传输种类
  + 第一种，点对点链路 Point-to-Point Links
    + 包含一个 Sender 和一个 Receiver
    + 点对点传输协议 Point-to-Point Protocol (PPP)
    + 高层数据链路控制 High-Level Data Link Control (HDLC)
  + 第二种，广播链路 Broadcast Links
    + 在同一个单一的广播频道上，包含多个 Sender 和多个 Receiver
    + Ethernet 和 LAN 就属于 Broadcast Links
    + 局域网 LAN —— 地理位置相对集中的网络，比如一个大楼、公司或者校园
    + Channel 会广播要发送出去的 Frame，然后每个 Node 都会收到一份 copy
+ 多端访问频道 Multiple Access Channels / Broadcast Channels —— 可以是线路链接、无线链接，或者卫星系统，当然还有排队现场也是一种
  + 现实生活中，一个广播频道内可以有上千个 Node 组成
  + 理想中的 Broadcast Channel 的特性：（假设这个通道的速度是 `R` Bits per Second）
    + 当只有一个 Node 在发送数据，吞吐量为 R bps
    + 当有 `M` 个 Nodes 在发送数据，每个 Node 的平均吞吐量为 `R/M`
    + 整个通道是去中心化的（Decentralized）
    + 通道中的协议是简单、易实现的
+ 多端访问难题 Multiple Access Problem —— 如何在一个共享的广播频道内协调和访问多个发送端和接收端，每一个 Node 都有可能成为发送端或者接收端
  + 类比 —— 派对、教室；空气是广播媒介，教授和学生是一个个 Node，他们都有可能说话、提问、或者回答，他们需要规则，比如举手来代表自己将要成为发送端、别人说话的时候不能打断、别人和你说话的时候别睡着等等
  + 链路帧冲突 Frame Collision —— 因为每个 Node 都可以成为发送端，当两个或者两个以上的 Nodes 在发送 Frame，那么可能出现 Frame Collision，每个 Nodes 会同时收到多个 Frame，因为不能做出正确选择，从而 Frames 被丢弃，造成通道的浪费
  + 所以需要研究 **多端访问协议 Multiple Access Protocols**
  + 多端访问协议 Multiple Access Protocols —— 一种规范了 Nodes 该怎样在广播频道内传输 Frame 的协议

![image-20210822174512007](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/image-20210822174512007.png)

+ **多端访问协议 Multiple Access Protocols 的分类** —— 为了解决 **链路帧冲突 Frame Collision**，过去 40 年有很多研究员和博士提出了很多种协议；不过这些协议可以大致分为三类
  + 通信通道划分协议 Channel Partitioning Protocols
  + 随机访问协议 Random Access Protocols
  + 轮流协议 Taking-Turns Protocols

## 通信通道划分协议 Channel Partitioning Protocols

![image-20210823001525001](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/image-20210823001525001.png)

+ 有三种通道划分协议
  + 时分多路复用 Time-Division Multiplexing (TDM)
  + 频分多路复用 Frequency-Division Multiplexing (FDM)
  + 码分多址 Code Division Multiple Access (CDMA)

+ **时分多路复用 Time-Division Multiplexing (TDM)** —— 假设一个通信通道支持N 个 Nodes，通道的传输速率是 R bps
  + TDM 方法将时间分为多个时间帧（Time Frames），然后再将时间帧分为 `N` 个槽（Slot），每个 Node 会被分配其中一个槽
  + 时间槽（Time Slot）的长度会允许一个 Frame 数据被完整的传送完成
  + 当每个 Node 都过完之后，会从头开始进入新的 Time Frame，然后每个 Node 继续有机会成为发送端
  + 优点
    + 消除了链路帧冲突 Frame Collision
    + 让每个 Node 都有机会成为发送端
  + 缺点
    + 如果一个通道里面有 `N` 个 Node 但只有一个 Node 需要发送数据，那么它的传输速率被限定在了 `R/N` 之内，并且只能等到它的 Time Slot 才能够发送数据

+ **频分多路复用 Frequency-Division Multiplexing** —— 假设一个通信通道支持N 个 Nodes，通道的传输速率是 R bps
  + FDM 方法将频率分为 N 个频道，每个频道的带宽是 R/N，每一个 Node 都被分配到一个频道上面
  + 优点
    + 消除了冲突
    + 相比 TDM，每个 Node 都有自己的通信频道，不需要花费时间等自己的 Time Slot
  + 缺点
    + 更小的带宽
    + 和 TDM 的缺点一样，一旦只有一个 Node 需要发送数据，那么它的带宽就被限制在了 R/N

