# Advanced Optimization 6 - Multiporting and Banking

## Why need Multiple Ports for Cache?

![Limitation of pipe with one-port cache](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Limitation%20of%20pipe%20with%20one-port%20cache.PNG)

+ 如果处理器是超标量，比如上图两个 pipe，那么 single-port Cache 只能支持一个 pipe 来运行 Mem 相关的指令（Load & Store），造成 One memory instruction per cycle 的吞吐量瓶颈
+ 如何让两个 pipe 都能同时访问内存呢？给 Cache 增加 ports

## Multiported Cache (Bad)

![Pipe with multiported cache](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Pipe%20with%20multiported%20cache.PNG)

+ 上图的 Cache 拥有两组 ports，虽然支持 Multiported SRAM 真实存在，但问题随之而来：
  + 虽然支持两组 ports，但面积增大了两倍，导致访问速度变慢
  + 如果处理器新增了一条流水线，那么这个 Cache 需要三组接口，更难设计（not scalable）
  + 如果两个 Store 指令在相同的地址存数，或者在相同 Clock Cycle 里面的一个 Load 指令和一个 Store 指令拥有相同的地址，那么会造成冲突
+ 如何解决？—— Banked Cache

## Banked Cache

![Banked Cache](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Banked%20Cache.PNG)

+ Banked Cache - 将 Address Space 分成几块
+ 利用 Portion of Address 来选择哪一个 Bank
  + 如果只利用 Address 的高位：如果我们连续访问一个 `array`，那么它们就享有相同的高位地址
  + 如果只利用 Address 的低位：如果我们连续访问一个 `array of struct` 的相同的元素，它们就会享有相同的地位地址
  + 所以高低位都可以用来判断
  + 在现代的处理器设计中，Banked Cache 都会有一个 Router/Crossbar 来选择哪一个 Bank 来访问，每一条 pipe 都可以访问所有的 Bank

+ 挑战
  + Extra Wiring for Router and Crossbar -> more delays
    + 如果使用更大的 Router/Crossbar 设计，会增加 delay
    + 如果使用较小的 Router/Crossbar 设计，就会降低 Bandwidth，因为 Bank Conflict 增多
  + Bank Conflict and Uneven Utilization
    + 使用其中一个 bank 的次数多于其他 bank
    + 多半是由于 Bank Conflict 导致