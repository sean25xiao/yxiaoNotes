# C++ Smart Pointer 1 - Why need and What is Smart Pointer?

## What is Wrong with Raw Pointers?

+ C++ Raw Pointer 需要自己手动 Allocate 和 Deallocate 内存
+ 容易产生 Memory Leak 等问题
+ 所以需要一个智能 Pointer 来自动化管理内存
  + 这个只能 Pointer 能够知道 Who owns the pointer
  + 这个只能 Pointer 能够知道 When should a pointer be deleted?

## What is Smart Pointers

+ Smart Pointer is object, 只能指向 heap-allocated memory
  + 需要 `#include <memory>`
  + 可以想成是一个 wrapper around a raw pointer
  + 定义为了一个 class template
    + 支持 Overloaded Operators
      + Dereference (`*`)
      + Member Selection (`->`)
      + But not support pointer arithmetic (`++`, `--`, etc.)
    + 可以有自定义的 deleter
+ Smart Pointer 可以自动删除，当不再需要时
+ 遵循 **RAII** 原则 - **Resource Acquisition Is Initialization**
  + 描述了一个 container object 的 lifetime 生命周期
  + **Resource Acquisition**
    + Open a file
    + Allocate memory
    + Acquire a lock
  + **Is Initialization**
    + The resource is acquired in a constructor
  + **Resource Relinquishing** - Happens in the destructor
    + Close the file
    + Deallocate the memory
    + Release the lock
  + RAII objects are allocated on the stack
+ C++ Smart Pointer 种类
  + Unique Pointers (`unique_ptr`)
  + Shared Pointers (`sharedd_ptr`)
  + Weak Pointers (`weak_ptr`)
  + Auto Pointers (`auto_ptr`) -- 已经过时了，尽量不要用

```c++
// A simple example of smart pointer
{
	std::smart_pointer<Some_Class> ptr = ......
	
	ptr->method();
	cout << (*ptr) << endl;
}

// ptr will be destroyed automatically when
// no longer needed
```

