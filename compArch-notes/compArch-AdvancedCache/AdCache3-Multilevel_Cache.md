# Advanced Optimization 3 - Multilevel Cache System

 

## Why need Multilevel Cache

+ 存储器技术有一个限制 - 一个内存不能做到同时运行速度快，而且存储容量又大

+ 为了解决这一点，可以用 Multilevel Cache 来模拟出一个又快同时容量又大的 Memory System

![Multilevel Cache System](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Multilevel%20Cache%20System.png)

## Multilevel Cache System

+ L1 Cache - 通常是一个 Small，Write Through Cache
	+ 设计成小型的 Cache 是因为它更加接近于 CPU，可以让 CPU 的 Clock Cycle 更快，更好的融合进 CPU 的 Pipeline 
	+ 设计成 Write Through Cache 是因为这样 L1 Cache 可以始终更新 L2 Cache
+ L2 Cache - 通常是一个 Large，Write Back Cache
	+ 设计成大型的 Cache 是因为想让它能够存储更多的数据，这样可以尽量减少 Miss Penalty
	+ 设计成 Write Back Cache 是因为这样可以减少 L2 Cache 和 Memory 之间的通信频率
	+ L2 Cache 的出现也可以简化 L1 Cache 的设计。比如，可以将 Error Recovery 交给 L2 Cache 完成
		+ 如果芯片被高能射线照射，高能粒子使 L1 Cache 的数据被修改，L1 Cache 可以用 Parity Bits 来告诉 L2 Cache，自己的数据错误了；L2 Cache 有很强的错误保护和恢复能力，它能够根据 L1 Cache 送来的 Parity Bits 随时 Invalidate L1 Cache 中的错误的数据，所以 L1 Cache 的设计更简单了

## Multilevel Cache Performance

+ 根据之前的 AMAT 公式，我们可以写出 L1 Cache 的 AMAT

  $$ AMAT_{L1}~=~Hit~Time_{L1}~+~Miss~Rate_{L1}~\times~Miss~Penalty_{L1} $$

+ 并且，L1 Cache 的 Miss Penalty 又和 L2 Cache 的 AMAT 有关
  $$ Miss~Penalty_{L1}~=~AMAT_{L2}~=~Hit~Time_{L2}~+~Miss~Rate_{L2}~\times~Miss~Penalty_{L2} $$

+ 所以，整个 Multilevel Cache System 的 AMAT 如下
  $$ AMAT_{total}~=~Hit~Time_{L1}~+~Miss~Rate_{L1}~\times~(~Hit~Time_{L2}~+~Miss~Rate_{L2}~\times~Miss~Penalty_{L2}) $$

+ 所以有了两个概念
	+ Local Miss Rate
	+ Global Miss Rate

## Local Miss Rate for Multilevel Cache

+ Local Miss Rate - The ratio of number of misses in Local Cache and the number of accesses to Local Cache
$$ Misses~in~Local~Cache~\div~Accesses~to~Local~Cache $$
+ 对于 L1 Cache 来说，Local Miss Rate 就是 L1 Cache 自己的 $Miss~Rate_{L1}$
+ 对于 L2 Cache 来说，Local Miss Rate 就是 L2 Cache 自己的 $Miss~Rate_{L2}$

## Global Miss Rate for Multilevel Cache

+ Global Miss Rate - The ratio of number of misses in cache and the number of CPU memory accesses 
+ 对于 L1 Cache 来说，Global Miss Rate 就是它的 $Miss~Rate_{L1}$
+ 对于 L2 Cache 来说，Global Miss Rate 是 $Miss~Rate_{L1}~\times~Miss~Rate_{L2}$

## Miss Per Instruction in Multilevel Cache

+ Miss Per Instruction - The ratio of Misses in Cache and the number of Instructions
$$ Misses_Per_Instruction~=~Misses~in~Cache~\div~Number~of~Instruction $$

## Two Inclusion Policies in Multilevel Cache

### Inclusive Multilevel Cache

+ 任何存在 L1 Cache 的数据都在 L2 Cache 里
+ 优点：在 Cache Coherence 里，External Snoop Access 只需要检查访问 L2 Cache 即可 
+ 如果有数据在 L1 Cache 或者 L2 Cache 被驱逐，那么数据就会被放回内存，同时更新 L1 Cache 和 L2 Cache

### Exclusive Multilevel Cache

+ L2 Cache 可能会有 L1 Cache 没有的数据
+ 好处：可以让整个 Multilevel Cache System 的总容量变大，并且可以最大程度利用 Temporal Locality，因为被 L1 Cache 驱逐的数据还会存在 L2 Cache，如果再次用到，不用去内存里再拿数据了
+ 用这种 Policy 需要一种 Swap Operation，可以在 L1 Cache 和 L2 Cache 之间交换数据
+ Example - AMD Altholon 运用了 64KB L1 Cache + 256KB L2 Cache，并且运用了 Exclusive Policy；所以总容量为 $64KB~+~256KB$，如果会用 Inclusive Policy，总容量只有 $256KB$

## Example of Multilevel Cache System

![Itanium-2 On-Chip Caches](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Itanium-2%20On-Chip%20Caches.png)

+ Example - Itanium-2 On-Chip Cache System
+ 有关 Multilevel Cache Size 的实验经验 ：
$$ L2~Cache~=~8~\times~L1~Cache $$
$$ L3~Cache~=~8~\times~L2~Cache $$
+ 这样选择 Size 的原因 - 需要在两方面寻找平衡
	+ 添加更多的 Cache Level 然后从里面找数据
	+ 或者还是直接进 Memory 里面找数据
+ 选择合适的 Size 可以平衡这两方面