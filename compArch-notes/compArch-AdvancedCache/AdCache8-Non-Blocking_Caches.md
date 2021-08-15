# Advanced Optimization 8 - Non-Blocking Caches

+ Non-blocking Cache, 别名 Out-of-order Cache，Lookup Free Cache
+ Idea - 如果遇到 Cache Miss，后续的 Mem 指令依然可以执行
  + Miss -> Miss (Concurrent miss) (Miss-under-Miss)
  + Miss-> Hit (Hit-under-miss)
+ 这个思路可以同时用在 In-order 或者 OOO 处理器上

![image-20210815105120416](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/image-20210815105120416.png)

+ 挑战
  + 需要添加额外的机制来确保乱序的 memory miss 能够按照正确顺序返回数据
  + 如果 Load 或者 Store 发生在已经 miss 的地址，Cache 系统很有可能出错
+ 如何解决？
  + 添加一块新的模块 ——  Miss Status Handling Register (MSHR) / Miss Address File (MAF)
    + `Block Addr` ---- The address of the cache block in the memory system
    + `Issue` ---- Get issued? 设立这个是因为 Memory 并不是在 miss 之后马上去寻找数据，比如系统的 Bus 正在忙碌其他事情
  + 另一块新的模块 —— Load/Store Entry Table
    + MSHR Entry ---- A pointer points to MSHR/MAF
    + Offset ---- Offset in the cacheline
    + Type ---- Memory 指令的种类
    + Destination ---- 当前的指令要写到哪个 Register，或者是 Memory Location / Store Buffer

![image-20210815110705027](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/image-20210815110705027.png)

+ 例子
  + 假设当前有两个 Load Misses，一个的地址是 0，另一个的地址是 10，假设 Cache Line 的长度是 16B（32位，每个 Cache line 能放四个数据，所以是 32 * 4 / 8 = 16）
    + 所以这两个地址是在同一个 Cache Line 上的
  + 这两个 Miss 会在 L/S Entry Table 里面填写上各自的两行信息，其中 MSHR Entry 都指向同一行 MSHR，因为它们都在同一个 Cacheline 上面，但 Offset 会有所不同
+ Non-blocking Cache Operation 流程
  + 当出现 Cache Miss 的时候
    + 检查 MSHR 有没有对应的地址
      + 如果有，那就在 L/S Entry Table 里面分配一个 entry，然后指向 MSHR
      + 如果没有，那么分配一个新的 MSHR 和 L/S Entry Table
      + 如果 MSHR 或者 L/S Entry Table 满了，那就 stall 整个系统
  + 当数据从 Memory 返回时
    + 找到正在等待的对应的 Load 或者 Store 指令
    + 将数据传送给处理器或者存在 Cache 里面
    + 完成之后，de-allocate MSHR entry 和 L/S Entry Table