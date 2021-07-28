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