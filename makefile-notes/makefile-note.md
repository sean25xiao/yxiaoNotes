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

```makefile
all : test

test : test1.o test2.o test3.o 
	gcc test1.o test2.o test3.o -o test

test1.o : test1.c
	gcc -c test1.c
	
test2.o : test2.c
	gcc -c test2.c
	
test3.o : test3.c
	gcc -c test3.c
	
clean :
	rm *.o
```

# Variables in Makefile

```makefile
# Makefile to demo the use of variable

src:=test.c  # Declare a variable called src

all:test

test:${src}
	gcc ${src} -o test
```

# Including another Makefil

```makefile
# Demo of including another makefile

all : test

test : test1.o test2.o test3.o
	gcc test1.o test2.o test3.o -o test

include test1.mk  # another makefile

include test2.mk
 
include test3.mk

include clean.mk
# test1.mk
test1.o : test1.c
	gcc -c test1.c

# test2.mk
test1.o : test2.c
	gcc -c test2.c

# test1.mk
test3.o : test3.c
	gcc -c test3.c

# clean.mk
clean :
	rm *.o
```

# Naming of Makefile

- 通常来说，

  ```
  make
  ```

   会根据一下顺序来搜寻需要运行的 makefile 脚本

  - `GNUmakfile` → 不经常使用
  - `makefile`
  - `Makefile` → 使用最多

- 或者也可以自定义 `makefile` 的名字

  - 使用 `make -f filename`   或者 `make --file=filename`  来执行

```bash
make -f Makefile_variable
make --file=Makefile_variable
```

# Wildcard

- `source = *.cpp` WRONG!   ↔ `source := $(wildcard *.cpp)` CORRECT!

```makefile
# Wildcard pattern
# $(wildcard *.c)

src := $(wildcard *.c)

all : test

test : ${src}
	gcc ${src} -o test
```

# Automatic Variables

- 目的：Get the info of target and prerequisites

```makefile
all : test

test : test1.o test2.o test3.o
	gcc $^ -o $@    # $@ means the target which is test here
					        # $^ means all files in prerequisites which is test*.o here

test1.o : test1.c
	gcc -c $<       # $< means first file in prerequisites which is test1.c here

test2.o : test2.c
	gcc -c $<

test3.o : test3.c
	gcc -c $<

clean :
	rm *.o
```

- 其他的 Automatic Variable
  - `$?` → The prerequisite files which have changes
  - `$*` → Target filename without suffix

# Search in Makefile

- 目的：如果有一些 source file 在其他文件夹，makefile 允许用户自定义一个文件，make 会来自动搜寻
  - VPATH - is used to keep the values of directories for search
    - `VPATH=src:config:includes` Search in `./src`, `./config`, and `./includes` files
  - vpath directive - Similar to VPATH, but more selective, allows you to specify a search path for a particular class of file names: those that match a particular pattern
    - vpath pattern directories → speciify the search path directories for file names that match the pattern

```makefile
VPATH=src

all : test

test : test1.o test2.o test3.o
	gcc $^ -o $@    # $@ means the target which is test here
					        # $^ means all files in prerequisites which is test*.o here

test1.o : test1.c
	gcc -c $<       # $< means first file in prerequisites which is test1.c here

test2.o : test2.c
	gcc -c $<

test3.o : test3.c
	gcc -c $<

clean :
	rm *.o
vpath %.c foo
vpath %   blish
vpath %.c bar

# This will look for a file ending in '.c' in foo, then blish, then bar
```

# Phony Targets

- Phony targets are used as target, should not be file
- Example:
  - we have clean:  `rm *.o`
  - if we have a file called `clean`, then make clean will always compile the file, and if it is updated, `rm *.o` will not be executed

```makefile
.PHONY : clean

clean:
	rm *.o
```

- We also can have target task as prerequisite for phony target

```makefile
.PHONY : install updateclean

install : clean
	tar -xvf sysman1.0.tar

update :
	cp ./updates/* ./sysman/patches

clean :
	rm -r ./sysman/*
```

# Variables in Make and Shell

- 在 Makefile 里面，一个 `$` 代表 variable in make，两个 `$$` 代表 variable in shell

```makefile
FILES=file1.c file2.c file3.c

all:
	for file in $(FILES); do \\
		echo $$file; \\
	done
```

- 在传递到 Shell 之后，Shell 看到的是这样的

```bash
for file in file1.c file2.c file3.c; do \\
	echo $file; \\
done
```

# Display the commands in make

- `@` - Suppress the echoing of that line
- `-n` or `—-just-print` - only prints the command line, does not execute
- `-s` or `—-slient` - Suppress echoing of all command lines

```makefile
all : test

test :
	@echo command line1
	echo command line2
	echo command line3
	
# In shell
$ make
command line1
echo command line2
command line2
echo command line3
command line3

$ make -n     # or make --just-print
echo command line1
echo command line2
echo command line3

$ make -s     # or make --slient
command line1
command line2
command line3
```

# Command Error

- `-` - ignores the error for that command
- `-i` or `—-ignore-errors` - Ignores the errors for command line of all rules
- `-k` or `--keep-going` - Ignores the errors for generating prerequisites target

# Use make in command (for subsystem make)

```makefile
# Method 1
subsystem:
	cd subsys && $(MAKE)

# Method 2
subsystem:
	$(MAKE) -C subsys
```

# Pass flags to subsystem make

```makefile
make -i
# in make file
subsystem:
	cd subdir && $(MAKE) MAKEFLAGS=i

# MAKEFLAGS does not get any value in the following cases
subsystem:
	cd subdir && $(MAKE) MAKEFLAGS=
```

# Display the directory information

- `-w` or `—-print-directory` - will print the directory information