# Introduction to the Link Layer

+ Terminology 
  + Node 节点 - Any device that runs link-layer protocol (Layer 2)
  + Link 链路 - Communication channels that connect adjacent nodes along the communication path
  + Link-Layer Frame 数据链路层帧 - The nodes encapsulate the datagram within a **Link-Layer Frame** and transmitted the frame into the link
    + Data Field 数据位 - 由被封装的 Datagram 构成
    + Header Field 头部位 
    + Frame 的具体结构由 Link-Layer Protocol 决定



# Example

![Link-Layer Example between host and server](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Link-Layer%20Example%20between%20host%20and%20server.PNG)

+ Host --> Server in Enterprise Network, 传输过程经过了 6 个 Links
  + Sending host to WiFi Access Point via WiFi Link
  + WiFi Access Point to Link-Layer Switch via Ethernet Link
  + Two Routers via link
  + The Router --> Link-Layer Switch via Ethernet Link
  + Switch --> Server via Ethernet Link



# Services Provided by Link Layer

+ Framing 帧化 - 请参考 <Link-Layer Frame 数据链路层帧>
+ Link Access 链路访问 - 通常 Medium Access Control (MAC) 会规定哪一个 Frame 可以传送到 Link
  + 对于 Point-to-Point Link 的情况，MAC 就可以不用存在了，因为 Link 两端只有一个 Sender 和 Receiver，这种情况下只要 Link 是 idle 状态，Sender 就可以发送 Frame
  + 对于多个 Nodes 共享一个 Broadcast Link 来说（也成为 Multiple Access Problem），需要 MAC 协议来协调这么多 Nodes 的 Frame 的传送
+ Reliable Delivery 可靠传输 - Link-Layer Protocol 需要保证封装在 Frame 里面的 Datagram 在 Link 上传输的时候不会存在 Error
  + 这种可靠传输可以设计的像 TCP 那样通过 Acknowledgments 和 Retransmissions 来解决
  + 也可以直接在本地，也就是在容易发送错误的 Link 上校正；这种很适合容易造成错误的 Link，比如 Wireless Link
  + 但有时，这种在链路上的可靠传输机制会给不经常犯错的 Links 造成不必要的 Overhead，比如 Fiber、Coax、和一些 Twisted-pair Copper Links；基于这个原因，一些 Wired Link-Layer Protocols 不会提供可靠传输机制
+ Error Detection and Correction 错误检查和校正
  + 有时 Receiver 会错误地把二进制数 1 看成 0，原因是一些信号衰减和电磁噪音
  + Sender 会在 Frame 里面包含 Error-Detection Bits，Receiver 会进行 Error Check
  + 通常 Link-Layer 的 error detection 在硬件上实现
  + Receiver 不仅仅发现 Error，还会定位错误发送的位置，并且校正错误



# Where is the Link Layer Implemented?

+ 通常情况下，Link Layer 同时是现在软件和硬件上
+ 软件上
  + 在发送端，实现在软件里的 Link Layer 主要负责：
    + 收集 Link-Layer 地址信息
    + 激活并且配置 NIC 硬件控制模块
  + 在接收端，实现在软件里的 Link Layer 主要负责：
    + 系统的中断
    + 处理错误情况
    + 将收到并且提取出来的 Datagram 送给上层的 Network Layer
+ 硬件上
  + Link Layer 实现在一个硬件模块上，名叫 Network Adapter，或者叫做 Network Interface Card (NIC)
    + 现代的 NIC 基本都装进了 Host 的主板中，或者集成进了 SoC 中
    + 比如：Intel 710 Adapter 实现了 Ethernet 协议
    + Atheros AR5006 实现了 802.11 WiFi 协议
  + 在发送端，网络硬件主要负责：
    + 将由上层协议/层面创建的 Datagram 包装在 Link-Layer 的 Frame 中
    + 之后按照链路访问协议（Link-Access Protocol）将 Frame 传输给通讯链路（Communication Link）
  + 在接收端，网络硬件主要负责：
    + 接受发送而来的 Frame，并提取出当中的 Network-Layer Datagram
  + 如果 Link Layer 要求错误检测（Error Detection）
    + 那么发送端会在 Frame 的 Header 设置 Error-Detection Bits
    + 接收端会来检测错误
+ 所以，Link-Layer 在计算机网络系统中，处在软件和硬件交界处，这一点很像 Computer Architecture