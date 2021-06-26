# Simple Makefile

```makefile
# This is a single line comment
# This is a multiple line \\
comment

all:test

test:test.c
	gcc test.c -o test
```

- 两种方法能让make file重新编译
  - 源文件更改
  - executable file 被删除

# Makefile Rule

```makefile
target: prerequisites/dependencies....   # mostly source file
	recipe/commands  # how to generate the executable output
target1: prerequisites/dependencies....   # mostly source file
	recipe/commands  # how to generate the executable output

target2: prerequisites/dependencies....   # mostly source file
	recipe/commands  # how to generate the executable output
	......
	......

target3: prerequisites/dependencies....   # mostly source file
	recipe/commands  # how to generate the executable output
	......
	......

targetN: prerequisites/dependencies....   # mostly source file
	recipe/commands  # how to generate the executable output
	......
	......

# Each target will be executed
```