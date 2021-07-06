# C++ Smart Pointer 3 - Shared Pointer

## Introduction

+ Shared Pointer -- 一种 Smart Pointer 的种类，可以让多个 pointer 指向一个 Heap object
+ `shared_ptr<T>`
  + Not unique -- 可以有很多个 `shared_ptr` 指向同一个 object
  + 多个 pointer 共享 ownership
  + Can be assigned and copied
  + Can be moved
  + 默认情况下，不支持 manage arrays
  + 运作原理：
    + 每创建一个关于这个 object 的 `shared_ptr`，都会把一个 counter 进 `1`。如果 counter 最后变为 `0`，就说明没有 `shared_ptr` 指向它了，被指向的 object 就可以安全删除了

## `shared_ptr` - Creating, Initializing, and Using

```c++
{
	std::shared_ptr<int> p1 {new int {100}};
	
	std::cout << *p1 << std::endl;   // 100
	
	*p1 = 200;
	
	std::cout << *p1 << std::endl    // 200
	
} // automatically deleted
```

## `shared_ptr` - Some Other Useful Methods

```c++
{
	// use_count - the number of shared_ptr objects managing the heap object
	std::shared_ptr<int> p1 {new int {100}};
	std::cout << p1.use_count() << std::endl;   // 1
	
	std::shared_ptr<int> p2 { p1 };   // shared ownership, using copy constructor of p1
	std::cout << p1.use_count() << std::endl;   // 2
    
    p1.reset();    // clear out p1, decrement the use_count; p1 is nulled out, but object is not destroyed since p2 is still pointing to it
    std::cout << p1.use_count() << std::endl;   // 0
    std::cout << p2.use_count() << std::endl;   // 1
    
}  // automatically deleted
```

## `shared_ptr` - User Defined Classes

```c++
{
	std::shared_ptr<Account> p1 {new Checking{"Larry", 500}}; 
	std::cout << *p1 << std::endl;   // display 500 (assume << has been overloaded)
	
	p1->deposit(1000);
	p1->withdraw(500);
	
} // automatically deleted
```

## `shared_ptr` - Vector and Move

```c++
{
	std::vector<std::shared_ptr<int>> vec;  // create a vector of shared_ptr with int type
	
	std::shared_ptr<int> ptr {new int{100}};
	
	vec.push_back(ptr);   // OK - copy is allowed!
	
	std::cout << ptr.use_count() << std::endl;   // 2, since ptr is copied to the vector
	
} // automatically deleted
```

## `shared_ptr` - `make_shared` in C++11

+ 尽量使用 `make_shared`，因为更加有效率
  + 可以将创建新 raw pointer、`use_count`、object 集合在一行代码上面
+ 当 `use_count` 变成 `0` 之后，存在 heap 上面的 object 就会被 deallocated

```c++
{
	std::shared_ptr<int> p1 = std::make_shared<int>(100);  // no need "new" keyword
    
    std::shared_ptr<int> p2 { p1 };    // use_count: 2
    
    std::shared_ptr<int> p3;
    
    p3 = p1;     // use_count: 3
	
} // automatically deleted
```