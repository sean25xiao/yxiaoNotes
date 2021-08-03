# C++ STL 4 - Iterator

## Types of Iterators

+ Input Iterators - it makes Container elements available to your program
+ Output Iterators - it can iterate over a sequence and write an element to a Container
+ Forward Iterators - it can iterate forward over a sequence and can rewrite any element
+ Bi-directional Iterators - like Forward Iterators, but they can iterate over a sequence in both directions
+ Random Access Iterators - it can use the subscript operator to directly access elements

## How to Declare Iterators

```c++
std::vector<int>::iterator it1;
std::list<std::string>::iterator it2;
std::map<std::string, std::string>::iterator it3;
std::set<char>::iterator it4;
```

+ Use vector as example

```c++
std::vector<int> vec {1,2,3};

 ______ ______ ______ ______
|   1  |   2  |   3  |      |
|______|______|______|______|
   |                     |
   |                     |
   |                     |
  vec.begin()           vec.end()
  first element         **location after last element**
  
 std::vector<int>::iterator it = vec.begin(); // in this case, 'it' is the pointer pointing to the first element of vec
 // or we can use this to simplify the syntax
 auto it = vec.begin();
```

## What Operations We Can Do on Iterators?

|   Operation   |                  Description                   |         Type of Iterator          |
| :-----------: | :--------------------------------------------: | :-------------------------------: |
|    `++it`     |      Pre-increment (move to next element)      |                All                |
|    `it++`     |     Post-increment (move to next element)      |                All                |
|  `it = it1`   |                   Assignment                   |                All                |
|     `*it`     |  Dereference (Get elem that it's pointing to)  |         Input and Output          |
|    `it->`     | Arrow operator (access attributes and methods) |         Input and Output          |
|  `it == it1`  |            Comparison for equality             |               Input               |
|  `it != it1`  |           Comparison for inequality            |               Input               |
|    `--it`     |                 Pre-decrement                  |           Bidirectional           |
|    `it--`     |                 Post-decrement                 |           Bidirectional           |
| `it+i, it+=i` |            Increment and decrement             | Random Access (ex. Array and Vec) |
|  `it < it1`   |                   Comparison                   |           Random Access           |

## Example of Iterator

+ Print at forward direction:

```c++
 std::vector<int> vec {1,2,3};
 std::vector<int>::iterator it = vec.begin();
 
 // Method 1
 while (it != vec.end()) {
 	std::cout << *it << " ";
	++it;
 }
 // Output: 1, 2, 3
 
 // Method 2
 for (auto it = vec.begin; it != vec.end(); it++) {
 	std::cout << *it << " ";
 }
 // Output: 1, 2, 3
```

+ Print at reverse direction:

```
std::vector<int> vec {1,2,3};
 
std::vector<int>::reverse_iterator it = vec.begin(); // now it pointers to the end of vec
 
while (it != vec.end()) {
 	std::cout << *it << " ";
	++it;   // Actually move backward
}
// Output: 3, 2, 1
```

## Other Iterators

+ `begin()` and `end()`
  + `iterator`
+ `cbegin()` and `cend()`
  + `const_iterator` - 不能改写它所指向的内容
+ `rbegin()` and `rend()`
  + `reverse_iterator`
+ `crbegin()` and `crend()`
  + `const_reverse_iterator`