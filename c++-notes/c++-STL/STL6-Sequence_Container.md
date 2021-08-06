# C++ STL 6 - Sequence Container

## `std::array`

+ Fixed Size
  + Size must be known at compile time
+ Direct element access
+ Use this instead of raw arrays when possible
+ `#include <array>`

```c++
std::array<int, 5> arr1 { {1, 2, 3, 4, 5} };  // C+11 needs two {{}}, C++14 don't need

std::array<std::string, 3> stooges {
    std::string{"Larry"},
    "Moe",
    std::string{"Curly"}
};

arr1 = {2, 4, 6, 8, 10};
```

```c++
std::array<int, 5> arr {1, 2, 3, 4, 5};
std::array<int, 5> arr1 {10, 20, 30, 40, 50};

std::cout << arr.size();  // 5

std::cout << arr.at();  // 1

std::cout << arr[1];    // 2

std::cout << arr.front();  // 1
std::cout << arr.back();   // 5

std::cout << arr.empty();  // 0 (false)
std::cout << arr.max_size();  // 5

arr.swap(arr1);   // swaps the 2 arrays
int *data = arr.data();   // get raw array address
```

## `std::vector`

+ Dynamic Size -  can grow or shrink size in run time
  + Elements are stored in contiguous memory as an array
+ Rapid insertion and deletion at the back (constant time)
+ Insertion or removal of elements in linear time
+ `#include <vector>`

```
    __________________
    |  1  |  2  |  3  |
    |_____|_____|_____|
       ^           ^
       |           |
       |           |
       front()     |
                  back()
```

```c++
vec.[0] = 100;  // no bound checking
vec.at(1) = 200; // bound checking

std::vector<Person> stooges;  // Person is a class

stooges.push_back(Person{"Yibang", 25});
stooges.emplace_back("Anqi", 3);  // Same as push_back(Person{....}), but more efficient!
                                  // No copy and no moves, just create object in vector container, use this!!!
```

```c++
std::vector<int> vec1 {1,2,3,4,5};
std::vector<int> vec1 {10,20,30,40,50};

auto it = std::find(vec1.begin(), vec1.end(), 3);
vec1.insert(it, 10);    // 1,2,10,3,4,5

it = std::find(vec1.begin(), vec1.end(), 4);
vec1::insert(it, vec2.begin(), vec2.end());
   // 1,2,10,3,10,20,30,40,50,4,5
```

+ Once `push_back` to a vector and exceeds its capacity, then its capacity will get double
  + `vec.capacity() x2`

## `std::deque`

+ Like a link list of vectors
  + `deque` uses discrete memory block, but each memory block has contiguous memory
+ Dynamic size
+ Direct element access
+ Rapid insertion and deletion at the back **and the front** (constant time)
+ Insertion or removal of elements (linear time)
+ `#include <deque>`

```
Person p1 {"Yibang", 25};
std::deque<Person> d;

d.push_back(p1);
d.pop_back();

d.push_front(Person{"Anqi", 18});
d.pop_front();

d.emplace_back("Boba", 1);  // add to back, it is very efficient!!!!
d.emplace_front("Boba2", 1);  // add to front, it is very efficient!!!!
```

## `std::list` and `std::forward_list`

#### `std::list`

+ Dynamic Size
+ Actually is a doubly-linked list
+ Direct element access is not provided!
+ Rapid insertion and deletion of elements anywhere in the container
+ Elements are not contiguous in memory
+ `#include <list>`

```
   ______           _______             ________
   |  1  | -------> |  2   |  --------> |  3    |
   |_____| <------- |______|  <-------- |_______|
     ^                                      ^
     |                                      |
     |                                      |
    front()                                back()
```

```c++
std::list<int> l {1,2,3,4,5};
std::list<int> l1 (10, 100);  // ten 100s

std::list<std::string> stooges {
	std::string{"Larry"},
	"Moe",
	std::string{"Curly"}
};

l = {2,4,6,8,10};
```

```c++
std::list<int> l {1,2,3,4,5};

std::cout << l.size();  // 5
std::cout << l.max_size();   // a very large number

std::cout << l.front();   // 1
std::cout << l.back();    // 5

// also support push_back(), pop_back(), push_front(), pop_front(), emplace_back(), emplace_front()
```

```c++
std::list<int> l {1,2,3,4,5};
auto it = std::find(l.begin(), l.end(), 3);

l.insert(it, 10);    // 1 2 10 3 4 5
l.erase(it);         // erases the 3,   1 2 10 4 5
l.resize(2);         // 1 2
l.resize(5);         // 1 2 0 0 0

// ==========================================
// Traversing the list (bi-directional)

std::list<int> l {1,2,3,4,5};
auto it = std::find(l.begin(), l.end(), 3);

std::cout << *it;     // 3
it++;
std::cout << *it;      // 4
it--;       // ok, since it is bi-directional
std::cout << *it;     // 3

```

#### `std::forward_list`

+ Same as `std::list` but only uni-directional
+ Has less overhead than `std::list`
+ Reverse iterators are not available

```
   ______           _______             ________
   |  1  | -------> |  2   |  --------> |  3    |
   |_____|          |______|            |_______|
     ^                                      
     |                                      
     |                                      
    front()                               No back()!!!
```

+ No `size()`, No `back()`, No `push_back()`, No `pop_back()`, No `emplace_back()`

```c++
std::forward_list<int> l {1,2,3,4,5};
auto it = std::find(l.begin(), l.end(), 3);

l.insert_after(it, 10);    // 1 2 3 10 4 5
l.emplace_after(it, 100);    // 1 2 3 100 10 4 5

l.erase_after(it);        // erases the 100,    1 2 3 10 4 5
l.resize(2);              // 1 2
l.resize(5);              // 1 2 0 0 0
```

