# C++ Smart Pointer 2 - Unique Pointer

## Introduction

+ 最简单的 Smart Pointer 的形式 —— Unique Pointer
+ `unique_ptr<T>`
  + Points to an object of type T on the heap
  + It is unique - 只能有一个 `unique_ptr<T>` 指向这个存在 heap 上的 object
  + It owns what it points to
  + Cannot be assigned or copied
  + Can be moved
  + 当 pointer 被 destroyed 的时候，它所指向的 object 也会自动 destroy

## `unique_ptr` - Creating, Initializing, and Using

```c++
{
	std::unique_ptr<int> p1 {new int {100}};
	
	std::cout << *p1 << std::endl;   // 100
	
	*p1 = 200;
	
	std::cout << *p1 << std::endl    // 200
	
} // automatically deleted
```

## `unique_ptr` - Some Other Useful Methods

```c++
{
	std::unique_ptr<int> p1 {new int {100}};
	
	std::cout << p1.get() << std::endl;   // 0x564388
	
	p1.reset();   // p1 is now nullptr
	
	if (p1)
		std::cout << *p1 << std::endl    // won't execute since p1 has been reset to NULL
	
} // automatically deleted
```

## `unique_ptr` - User Defined Classes

```c++
{
	std::unique_ptr<Account> p1 {new Checking{"Larry", 500}}; 
	std::cout << *p1 << std::endl;   // display 500 (assume << has been overloaded)
	
	p1->deposit(1000);
	p1->withdraw(500);
	
} // automatically deleted
```

## `unique_ptr` - Vector and Move

```c++
{
	std::vector<std::unique_ptr<int>> vec;  // create a vector of unqie_ptr with int type
	
	std::unique_ptr<int> ptr {new int{100}};
	
	vec.push_back(ptr);   // ERROR - copy not allowed
	
	vec.push_back(std::move(ptr));
    
	// ========
	std::unique_ptr<Test> t1 {new Test{100}};
    
	std::unique_ptr<Test> t2;
    
	t2 = t1;  // ERROR - assign not allowed
    
	t2 = std::move(t1);  // OK, now t1's ownership of the object is moved to t2, and t1 now is a NULL pointer
    
	if(!t1)
		std::cout << "t1 is nullptr" << std::endl;  // it will print this
	
} // automatically deleted
```

## `unique_ptr` - `make_unique` in C++14

```c++
{
	std::unique_ptr<int> p1 = make_unique<int>(100);  // no need "new" keyword
	
	std::unique_ptr<Account> p2 = make_unique<Acccount>("Currly", 5000);  // can directly initialize the object here
	
	auto p3 = make_unique<Player>("Hero", 100, 100);  // "auto" keyword can let compiler deduce the type of p3 based on what make_unique returns
    
	// =================
	std::vector<std::unique_ptr<Account>> accounts;
    
	accounts.push_back( make_unique<Checking_Account>("James", 1000));
	accounts.push_back( make_unique<Savings_Account>("Billy", 4000, 5.2));
	accounts.push_back( make_unique<Trust_Account>("Bobby", 5000, 4.5));
    
	for (auto acc: accounts)  // ERROR - copy not allowed
	for (const auto &acc: accounts)
		std::cout << *acc << std::endl;
	
} // automatically deleted
```

