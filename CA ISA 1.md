# What is ISA?

+ ISA - Instruction Set Architecture ^4f58d2
	+ The abstract layer which can translate Application to Physical Devices
	+ The abstract layer provided to software which is designed not to change very much
+ Programmer can see ISA! 
	+ Does it have memory?
	+ How many registers it has?
	+ Defines operations -> Instructions and how they work
	+ What happens if interrupt and exception happened?
	+ Input and output
	+ Data Types and Sizes: 16-bit? 32-bit? 64-bit? Floating-Point?

# Who Determine ISA

+ Application Requirements 程序要求
	+ It suggests how to improve architecture
	+ 例子：Video Processing 需要更多 Vector 相关的指令
+ Technology Constraints 技术限制
	+ It restricts what can be done efficiently
	+ New technology makes new architecture possible

# Why Diversity in ISA?

- 受到技术影响
  - 存储非常的昂贵
  - 现在由了多核处理器技术
- 程序也影响了 ISA
  - 例子：DSP
  - 编译器也在变得更强，所以能够更有效地管理寄存器、内存等等
