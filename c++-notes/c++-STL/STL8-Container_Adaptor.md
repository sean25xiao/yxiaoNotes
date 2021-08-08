# C++ STL 8 - Container Adaptors

## `std::stack`

+ Last-in First-out Data Structure (LIFO)
+ Implemented as an adapter over other STL container, can be implemented as a vector, list or deque
+ All operations occur on one end of the stack (top)
+ No iterators are supported

+ `#include <stack>`

+ Operations
  + `push` - insert at the top of the stack
  + `pop` - remove from the top
  + `top` - access the top element of the stack
  + `empty` - is the stack empty?
  + `size` - number of elements in the stack

```c++
// Since the stack is an adaptor class, 
// we can choose what underlying container will be used
// when we create our stack obectjs

std::stack<int> s1;   // by default: deque

std::stack<int, std::vector<int>> s2; // vector

std::stack<int, std::list<int>> s3;   // list

std::stack<int, std::deque<int>> s4;    // deque
```

## `std::queue`

+ First-in First-out Data Structure (FIFO)
+ Implemented as an adapter over other STL container, can be implemented as a list or deque
+ Elements are pushed at the back and popped from the front
+ No iterators are supported

+ `#include <queue>`

Operations

+ `push` - insert at the back of the queue
+ `pop` - remove from the front of the queue
+ `front` - access the element at the front
+ `back` - access the element at the back
+ `empty` - is the queue empty?
+ `size` - number of elements in the stack

```c++
// Since the queue is an adaptor class, 
// we can choose what underlying container will be used
// when we create our stack obectjs

std::queue<int> q1;   // by default: deque

std::queue<int, std::list<int>> q2;   // list

std::queue<int, std::deque<int>> q3;    // deque
```

```c++
std::queue<int> q;

// back -------> front
q.push(10);   //  10

q.push(20);   //  20 10

q.push(30);   // 30 20 10

std::cout << q.front();   // 10
std::cout << q.back();    // 30

q.pop();    // 30 20,   10 is removed
q.pop();    // 30,   20 is removed

std::cout << q.size();   // 1
```

