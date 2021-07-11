# Advanced Optimization 4 - Victim Cache

## Why need Victim Cache

+ Example:
```C
int a[1M]
int b[1M]
int c[1M]

for(i=0; i=1M; i++) {
	c[i] = a[i] + b[i];
}
```

+ 假设 `c[], a[], b[]` 都被索引在一个相同的 Cache Line 上，并且这个 Cache 是个 Two-Way Associative
+ 其运行结果是，`a[], b[], c[]` 都互相冲突，产生 Conflict Miss，并不能很好的运用 Temporal Locality 和 Spatial Locality
+ 如何解决 - 使用 Victim Cache 来变相增大 Associativity
+ Victim Cache 可以减少 Miss Rate 和 Miss Penalty

## Features of Victim Cache

+ Victim - It means evicted Cache Line
+ 在传统的 Cache 中，Evicted Cache Line 会被 Invalidated 或者被新的 Cache Line 代替
+ 在 Victim Cache 系统中，Evicted Cache Line 会被存在另一个小的 Cache 中，被称为 Victim Cache
+ Victim Cache - 通常是 Fully Associative，拥有着很少的 Entries （通常 4~16 个）
	+ 可以和 L1 Cache 进行 Parallel Checking 或者 Series Checking
+ Victim Cache 的坏处 - 如果是并行 Checking，那么复杂度和功耗都会上升；如果是串行 Checking，那么 Latency 就会上升

## Victim Cache Work-flow

![Victim Cache Workflow](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Victim%20Cache%20Workflow.png)

1. 检查 L1 Cache；如果命中，那么返回数据
2. 如果地址没有命中，那么将会检查 Victim Cache（假设是串行检查）。如果 Victim Cache 里面有这个数据，那么该数据将会返回给 L1 Cache 然后再送还给处理器。在 L1 Cache 里被驱逐的数据，会存放进 Victim Cache 里面
3. 如果 L1 Cache 和 Victim Cache 都没有该数据，那么就会从 Memory 取出该数据，并把它放在 L1 Cache 里面。如果在该次操作里面，L1 Cache 需要驱逐一个数据，那么这个被驱逐的数据会被存进 Victim Cache 里面。任何被 Victim Cache 驱逐的未修改过的数据，都会写进 Memory 里面或者被丢弃

（L1 Cache 和 Victim Cache 的数据互相不包含，有点像 Multilevel Cache 的 Exclusive Policy）