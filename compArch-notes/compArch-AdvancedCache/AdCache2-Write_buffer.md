# Advanced Optimization 2 - Write Buffers

+ Reduce [[CA Cache 17 - Miss Penalty | Miss Penalty]]

## Why need Write Buffers

+ 如果有连续的 Store 指令（其实很普遍），我们不希望让 Cache 和 Memory 之间每次都通信，这样耗费的时间太多
+ 所以可以加一个Write Buffer，类似一个 Queue，可以 Queue-in 要存入内存的数据
+ 好处是，如果有连续的 Store 指令，他们可以在 Write Buffer 里形成一个完整的 Block，然后再存入 Memory 中，而不是每次都要访问内存，这种效果叫做 Write Merging
	+ 可以减少 Cache 和 Memory 之间的 Bandwidth 的压力，也能减少 Miss Penalty
+ 当 Store 指令把数存入 Write Buffer 之后，从处理器的角度来说，整个写入内存的过程已经完成

## Write Buffer for Write Through Cache

+ 对于 Write Through Cache 来说，Write Buffer 里面存了要写入内存的数据
+ 如果 Write Buffer 满了，那么处理器和 Cache 必须 Stall，等待 Write Buffer 清空；
+ 如果出现 Read Miss，那么可以比较 Read Miss 的地址和存在 Write Buffer 里面的地址；如果没有相同，可以允许 Read Miss 比之前的 Store 指令提前去内存拿数；如果有相同的地址，那么就把这个在 Write Buffer 里面的数据送还给 CPU

## Write Buffer for Write Back Cache

+ 对于 Write Back Cache 来说，Write Buffer 里面存了被驱逐（Evicted）的 Dirty Line，它们正在等待被写入
+ 如果出现 Read Miss，策略和上述 Write Buffer for Write Through Cache 是一样的