# Naive Register File

![Naive RegFile](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Naive%20RegFile.png)

+ Why it is not good?
	1. If it gets longer, for example, 1000 bits, need a larger MUX
	2. 从 Propagation Delay 角度来说，整个系统会有很大的延迟
	3. 从面积角度来说，不方便做成芯片，太长

+ 这就是为什么现代的 Memory 都要做成 Memory Array

# Memory Array - Register File

![MemArray - RegFile](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/MemArray%20-%20RegFile.png)

## 为什么做成 Memory Array

+ 对比上面提到的 Naive Memory，Memory Array 有一下几个优点：
	+ 正方形更好的优化了面积
	+ 正方形可以最小化 Propagation Delay
	+ Decoder 也不再是单独的一个 MUX，而是被分成了很多份

## Feature of Register File

+ 图中每个小的正方形都是一个 Bit-Cell
+ Row Decoder - 控制打开 Word Line
+ Column Decoder - 由多个小 MUX 组成，可以选择 Word Line 中的各个 Bit-Cell

# Memory Array - SRAM

![MemArray - SRAM](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/MemArray%20-%20SRAM.png)

+ SRAM - Static Random Access Memory 静态随机存储器
+ SRAM 通常用于制造 Cache

## Feature of SRAM
+ 和 Register File 使用的 Memory Array 技术很像
+ 但每个 Bit-Cell 确很不一样
+ 添加了 Operational Amplifier，用于分辨 `bit` 和 `bit_b` 线上电压的微小差异
+ `bit` 和 `bit_b` 通常可以被重复用，所以通常 SRAM 都是 Single-Port

# Memory Array - DRAM

![MemArray - DRAM](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/MemArray%20-%20DRAM.png)

+ Dynamic Random Access Memory - 动态随机存储器
+ 通常不和微处理器放在一起（built out of mico-processor）
+ 通常用于制作主存（Main Memory）

## Feature of DRAM
+ 和 Register File 使用的 Memory Array 技术很像
+ 但每个 Bit-Cell 确很不一样
+ 每个 Bit-Cell 只包含了一个 Transistor 和一个 Capacitor
+ DRAM 不能被普通的 CMOS 制程制造出来，因为它不是纯逻辑电路，里面包含了电容，所以需要特制的 DRAM 制程
+ DRAM 的优点：
	+ 对比 SRAM 和 Register File，在相同的面积下，DRAM 可以储存更多的数据，因为每个 Bit-Cell 需要的晶体管非常的少
+ 因为电容会经常漏电荷，所以我们需要经常 refresh DRAM in a few seconds

# Size between DRAM and SRAM

![Size Compare of SRAM and DRAM](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Size%20Compare%20of%20SRAM%20and%20DRAM.png)

# Trade-off between Memory Technology

![Trade-off of Mem Technology](https://raw.githubusercontent.com/sean25xiao/yxiaoNotes-pic/main/Trade-off%20of%20Mem%20Technology.png)