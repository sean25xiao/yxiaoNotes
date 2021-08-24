# Makefile 1 Introduction

## What is Makefile

+ `make` Program - 可以阐述源代码和可执行文件之间的关系，并且进行自动转化/编译
+ 使用 Makefile 的好处
  + 节约时间
  + 减少编译的工作量

## Basic Syntax

+ 规则 Rule —— 描述了

```makefile
target: prereq1 prereq2
	commands
```

+ 目标 Target —— 需要建造的文件
+ 前置 Prerequisite —— 建造 Target 所需要的源文件（原材料）
  + 如果任何一个前置文件被更新了，Target 就会重新被建造一遍
+ 指令 Commands —— 建造 Target 所需要的指令（建造方法）
  + 指令通常会被传送到 subshell 进行执行
+ 

## Example

```makefile
count_words: count_words.o lexer.o -lfl
	gcc count_words.o lexer.o -lfl -o count_words

count_words.o: count_words.c
	gcc -c count_words.c   # 生成 .o 可重定位目标文件，用于之后的链接
	
lexer.o: lexer.c
	gcc -c lexer.c
	
lexer.c: lexer.l
	flex -t lexer.l > lexer.c
```

+ 检查 `count_words.o` 有没有被建造，发现没有
+ 转到第二个 Rule，检查 `count_words.c` 是否是另一个 Target，发现不是，并且存在，因为是源代码
+ 执行第二个 Rule 的指令，生成 `count_words.o` 文件
+ 回到第一个 Rule，往下检查，检查 `lexer.o` 有没有被建造，发现没有
+ 转到第三个 Rule，检查 `lexer.c` 是 Target 吗，发现是
+ 转到第四个 Rule，检查 `lexer.l`，发现其不是 Target，也存在
+ 执行第四个 Rule 的指令，生成 `lexer.c`
+ 回到第三个 Rule，执行其指令，生成 `lexer.o`
+ 回到第一个 Rule，检查第三个前置，发现是 `-lfl`，链接对于的 System Library
  + `-l` 可以提示链接器链接系统库，在这个案例中，`libfl.a` 将会被链接
  + 对于 `-l<NAME>` 通常 make 会搜索 `lib<NAME>.so`，如果没找到，会继续搜索 `lib<NAME>.a`
+ 最终执行第一个 Rule 的指令，生成 `count_words` 可执行目标文件
