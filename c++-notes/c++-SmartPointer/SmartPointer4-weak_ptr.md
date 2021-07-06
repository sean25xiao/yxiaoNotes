# C++ Smart Pointer 3 - Weak Pointer

## Introduction

+ Weak Pointer -- 一种提供 non-owning 关系的弱化 reference
+ `weak_ptr<T>`
  + 指向了一个储存在 heap 中的 Type T 的 object
  + 一直都是从 `shared_ptr` 创建过来的
  + 创立 `weak_ptr` 不会增加或者减少 `use_count`
  + 不会影响它们所指向的 object 的 lifetime
  + 用法：
    + Prevent strong reference cycles which could prevent objects from being deleted
      + A refers to B, and B refers to A
      + 有点像鸡生蛋还是蛋生鸡的问题
      + 这样循环的问题，会导致 Memory Leakage，因为其中 object 被 destroy 的话，另一个就没有指针指向它了，所以两个 objects 就会留存在 heap 中
    + 有时我们需要一个 pointer 来临时指向一个或者几个 object，比如一个 iterator pointer that traverses a list of nodes

```
      A                                      B
_______________                      _______________
| shared_ptr<B>|====================>|              |
|              |                     |              |
|              |<====================| shared_ptr<A>|
|______________|                     |______________|
```

+ 所以这种情况下，要使用 `weak_ptr`
  + 首先要决定谁拥有谁
  + 比如，我们决定 A 拥有 B
  + 那么 A 可以用 `shared_ptr` 来指向 B
  + 但 B 要用 `weak_ptr` 指向 A

```
      A                                      B
_______________                      _______________
| shared_ptr<B>|====================>|              |
|              |                     |              |
|              |<--------------------| weak_ptr<A>  |
|______________|                     |______________|
```

## Example

```c++
// Section 17
// Weak Pointers
#include <iostream>
#include <memory>

using namespace std;

class B;    // forward declaration

class A {
    std::shared_ptr<B> b_ptr;
public:
    void set_B(std::shared_ptr<B> &b) {
        b_ptr = b;
    }
    A() { cout << "A Constructor" << endl; }
    ~A() { cout << "A Destructor" << endl; }
};

class B {
    std::weak_ptr<A> a_ptr;     // make weak to break the strong circular reference
public:
    void set_A(std::shared_ptr<A> &a) {
        a_ptr = a;
    }
    B() { cout << "B Constructor" << endl; }
    ~B() { cout << "B Destructor" << endl; }
};

int main() {
    shared_ptr<A> a  = make_shared<A>();
    shared_ptr<B> b = make_shared<B>();
    a->set_B(b);
    b->set_A(a);
    
    return 0;
}
```

