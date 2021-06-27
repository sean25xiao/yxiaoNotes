# Advanced Optimization 1 - Pipelined Cache

+ Reduce Write Hit Time - 说不准，因为虽然减少了 Hit 时候的负担，但因为新增了 **Delay Cache Store Buffer**，也可能会增加 Write Hit Time

+ Increase Bandwidth

## Why need Pipelining Caches

![Process of Write to Cache](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Process%20of%20Write%20to%20Cache.png)

+ 写缓存有两步：
	+ 使用 Index 来索引定位 Tag Array （Tag Array 和 Data Array 分开的）
	+ 检查 Valid Bit 和 Tag；如果 Tag 相同，则写入缓存
+ 这两步可以同时做，但对 Clock Cycle 不是那么的友善
+ 如果把这两步用时序分开，那么 Memory Stage 会需要两个 Cycle
	+ 一个周期用来比较 Tag
	+ 一个周期用来写入数据（如果 Hit）
+ 所以可以使用 Pipelined Cache 来对这两个时序进行 Pipelining

## How Pipelined Cache Works

+ Instead of doing data store in M stage, only do Tag checks in M stage. Store the data in a Buffer, which is named as **Delayed Cache Store Buffer**.
+ 那这个 Buffer 什么时候写进 Cache ？当遇到下一个 Store 指令时候，当它在比较 Tag 的时候，上一个 Store 指令就可以完成第二步：Write to Cache

```tex
        0   1   2   3   4   5   6   7   8  
SW1     CHK WC
SW2         CHK -   -   -   -   WC
INTR1           F   D   E   M   W
SW3                             CHK WC

WC  = Write Cache
CHK = Check Tag
```

## Trade-off of Pipelined Cache

![Pipelined Cache More HW](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Pipelined%20Cache%20More%20HW.png)

+ 如果在 Store 后面跟着一个 Load 指令，那么就需要在 Delayed Stored Buffer 里面寻找有没有相同的地址
+ 如果有，就需要使用 Bypass 把值送还给 CPU
+ 上图的黄色路线，就是 Bypass 路线
+ Trade-off of Pipelined Cache - Add more HW such as MUX and more Comparator