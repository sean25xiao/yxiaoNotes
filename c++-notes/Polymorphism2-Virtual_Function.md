

## Virtual Function

+ Virtual Function can override a function or method into dynamically bounded
+ 只要在 Base Class 里面定义了，后面的 Derived Class 里面的都会被自动定义为 `Virtual`
	+ 不过最好还是加上 `Virtual`，这样方便阅读代码
+ Derived Class 里面的 method 必须要有完全相同的输入输出类型
+ 只支持 Pointer 指向 Base Class 的情况（使用 `New` 的情况）

## An example of Virtual Function

```c++

#include <iostream>

// This class uses dynamic polymorphism for the withdraw method
class Account {
public:
    virtual void withdraw(double amount) {  // make this as virtual function
        std::cout << "In Account::withdraw" << std::endl;
    }
};

class Checking: public Account  {
public:
    virtual void withdraw(double amount) {
        std::cout << "In Checking::withdraw" << std::endl;
    }
};

class Savings: public Account  {
public:
    virtual void withdraw(double amount) {
        std::cout << "In Savings::withdraw" << std::endl;
    }
};

class Trust: public Account  {
public:
    virtual void withdraw(double amount) {
        std::cout << "In Trust::withdraw" << std::endl;
    }
};

int main() {
    std::cout << "\n === Pointers ==== " << std::endl;
    Account *p1 = new Account();
    Account *p2 = new Savings();
    Account *p3 = new Checking();
    Account *p4 = new Trust();
    
    p1->withdraw(1000);
    p2->withdraw(1000);
    p3->withdraw(1000);
    p4->withdraw(1000);
    

    std::cout << "\n === Clean up ==== " << std::endl;
    delete p1;
    delete p2;
    delete p3;
    delete p4;
        
    return 0;
}

// Terminal output
In Account::withdraw
In Checking::withdraw
In Savings::withdraw
In Trust::withdraw
```

+ 但是最终结果会显示需要 Virtual Destructor

## Virtual Destructor

+ 如果一个 Class 有 `virtual` function，那么永远都要把这个 Class 的 Destructor 变成 `virtual`

```c++
class Account {
public:
	virtual void withdraw(double amount);
	virtual ~Account();
}
```

## Virtual Function —— Override Specifier

+ Derived Class 里面的 `virtual` method 如果想要 override Base Class 里面的 `virtual` method，需要拥有同样的 method signature：名字、Return Value、类型（比如是否为 `const`）
  + 如果不一样，那么编译器就会认为 Derived Class 里面的 method 是想 refine，而不是 override
  + 编译器就不会做 Dynamic Bound，而是 Static Bound

```c++
class Base {
public:
	virtual void say_hello() const {
		std::cout << "" << std::endl
	}
	virtual ~Base() {}
};

class Derived: public Base {
public:
	virtual void say_hello() {  // Notice I forgot the const - NOT OVERRIDING
		std::cout << "" << std::endl
	}
	virtual ~Derived() {}
};
```

+ 这个时候，编译器不会报错或者报 warning，这种 bug 很难查找
  + 有一个方法是在每个 virtual method 后面加 **Override Specifier**：`virtual void say_hello() override`
  + 这种情况下，编译器会报错，这样就能找到 bug 了

## The `final` Specifier

+ 如果 final 用在 class level —— 它可以阻止当前 class 被 derived
+ 如果 final 用在 method level —— 它可以阻止当前 method 被 derived class 重载掉（overridden）

```c++
class my_class final {  // my_class cannot be derived, comiling error
	...
};

class Derived final: public Base {  // Derived cannot be derived, comiling error
	...
};
```

```c++
class A {
public:
	virtual void say_hello();
};

class B: public A {
public:
	virtual void say_hello() final;  // prevent futher overriding
};

class C: public B {
public:
	virtual void say_hello();  // Can't override -- Compiling error
};
```

