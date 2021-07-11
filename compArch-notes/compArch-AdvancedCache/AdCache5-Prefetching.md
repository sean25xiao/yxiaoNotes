# Advanced Optimization 4 - Prefetching

## Introduction

![Prefetching](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Prefetching.png)

+ Prefetching - Speculate on future instruction or data accesses and fetch them early into Caches
+ Reduce Miss Rate, focusing on Compulsory Miss
+ Reduce Miss Penalty
+ Might hurt Bandwidth

## Why need Prefetching

+ 有很多方法可以减少 Capacity Miss 和 Conflict Miss，比如通过更多的 Associative 来降低 Conflict Miss 和通过使用更大的 Cache 来降低 Capacity Miss
+ 但目前，还没有一种方法可以降低 Compulsory Miss
+ 通过类似处理器的 Speculative Execution，我们也可以在 Cache 里做类似的操作 

## The Issues of Prefetching

+ Prefetching 可能会对 Capacity Miss 和 Conflict Miss 造成负面影响，因为可能提前 Fetch 的数据没有用上，反而污染了（Pollute）了 Cache Line
+ 所以 Prefetching 有两个要点
	+ Usefulness 实用性 - 应该带上来会被命中的数据
	+ Timeliness 及时性 - 应该在恰到好处的时间点带上来数据；如果太早，会被提早驱逐出去，对性能、功耗、Bandwidth 造成影响，这在 Multi-core 和 Off-Chip 系统里很严重；如果太晚，则预测的数据没有用上
+ 一般来说，Prefetching 在 Instruction Cache 里面很好用，因为通常情况下都是按顺序的执行指令
+ 并且可以用于 Instruction L1 Cache 和 L2 Cache 之间，因为他们是 On-Chip，Bandwidth 很大，
+ 不用于 L2 Cache 和 Memory 之间是因为 Memory 是 Off-Chip，中间的 Bandwidth 很昂贵

## Hardware Instruction Prefetcher

![HW Intruction Prefetcher](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/HW%20Intruction%20Prefetcher.PNG)

+ 实用案例：Alpha AXP 21064
+ 新模块 —— Stream Buffer —— 可以 Stream 下一条很有可能被执行的指令
+ 假设 CPU 请求了 `block i`，但未命中
  + 这种情况下，Cache 系统会拿取两个 Block
    + `block i` 放在 Instruction Cache 里面
    + `block i+1` 放在 Stream Buffer 里面
    + 执行完 `block i` 后，CPU 会请求 `block i+1` 的指令
    + L1 Instruction Cache 中未命中，但 Stream Buffer 命中
    + `block i+1` 会从 Stream Buffer 里面移到 L1 Instruction Cache，接着 Stream Buffer 会读取下一条 `block i+2` 指令
    + （以上假设这段代码中没有 branch 分支）

## Hardware Data Prefetcher

+ 有以下几种方案
+ **Prefetch-on-miss**
  + 和 上面讲的 Instruction Prefetcher 很像，如果 `block i` 未命中，那么同时拿取 `block i+1`
+ **One Block Lookahead (OBL)**
  + 不管 `block i` 有没有命中，都拿取 `block i+1`
  + 那么这和 doubling block size 有什么区别呢？
    + 有！Doubling block size 是在同一时钟周期里面拿两个 block，所以工作量更大，容易引起 Timing Violation；但 Prefetcher 是分步拿取的：拿上来 `block i` 先用，然后 Prefetcher 自己再去拿 `block i+1`, `block i+2`, ......, `block i+n`，这样可以将拿取这些 block 的延迟隐藏起来（hide latency）

+ **Strided Prefetch** （现代 Intel 处理器使用的方法）

  + 以上两种方法都不能解决一问题
    + 上面两个方法都假设是连续的数据，比如 `array`；但有时数据不是连续的，比如 `array of struct`，这时我们想连续提取每个 `struct` 里面相同的元素，就是以固定的 offset 去 `array of struct` 里面拿数，这时 memory access 就不是连续的了
  + 所以就需要一个更聪明的 Prefetcher 以一定的步长（stride）来拿数
    + 这个 Prefetcher 需要能发现规律，比如，如果发现连续两次取数都有 offset N 的偏移量，`block i`, `block i+n`, `block i+2n`，那么，Prefetcher 就会自动以这个规律去拿取 `block i+3n`, `block i+4n`，........

  + Strided Prefetch 还用在了 IBM Power 5 上面，这款处理器有八个独立的 Strided Prefetcher，每一个可以提前拿取 12 个 Block Line

## Software Prefetching

+ 利用软件来提前取数

```c++
for (i = 0; i < N; i++)
{
  prefetch(&a[i+1]);     // hint for HW to prefetch
  prefetch(&b[i+1]);
  sum = sum + a[i] * b[i];
}
```

+ SW Prefetching 最大的问题就是 Timing，软件需要在合适的时间拿取合适的数据
  + 根据运算指令的复杂程度，SW Prefetching 有可能过早或者过晚的拿到了数据
  + Programmer 和 Software 不知道真实的底层的 Memory load 是多大
  + Programmer 和 Software 不知道 Memory Controller 的阻塞情况
  + Programmer 和 Software 不知道数据 `a[i]` 和 `b[i]` 有没有命中
  + 上面这些都是处理器的动态信息，很难把握，所以 Timing 就成为了 Software Prefetching 的一大难题