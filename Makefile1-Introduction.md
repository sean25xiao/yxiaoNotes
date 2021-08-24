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

