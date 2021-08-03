# C++ STL 1 - STL Introduction

## What is STL?

+ A library of powerful, reusable, adaptable, generic classes and functions
+ Implemented using C++ templates
+ Implements common data structures and algorithms
+ Huge class library
+ Alexander Stepanov in 1994

## Why need STL

+ The algorithm provided in STL is known about time and size complexity
+ Re-usability - They have been tested by million lines of code
+ Consistent, Fast, and Type-safe
+ Extensible

## STL Elements

### Container

+ Containers - Collections of objects or primitive types
	+ Ex. Array, Vector, Deque, Stack, Set, Map, etc
+ Containers are data structures that can store object of almost any type
+ They are Template-Based Classes
+ Each container has Member Functions
	+ Some are specific to the container
	+ Others are available to all containers
+ Each container has an associated header file
	+ `#include <container_type>`

##### Types of Containers

+ **Sequence Containers** - the containers that maintain the ordering of inserted elements
	+ Array, Vector, List, forward_list, Deque
+ **Associative Containers** - insert elements in a predefined order or no-order
	+ `set`, `multi-set`, `map`, `multi-map`
+ **Container Adapters** - do not support iterators, so they do not use with STL algorithms
	+ Stack, Queue, Priority queue

##### Common Container Member Function

| Function                    | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| Default Constructor         | Initializes an empty container                               |
| Overloaded Constructors     | Initializes containers with many options                     |
| Copy Constructor            | Initializes a container as a copy of another conatiner       |
| Move Constructor            | Moves existing container to new container                    |
| Destructor                  | Destroys a container                                         |
| Copy Assignment (operator=) | Copy one container to another                                |
| Move Assignment (operator=) | Move one container to another                                |
| size                        | Returns the number of elements in the container              |
| empty                       | Returns boolean - is the container empty?                    |
| insert                      | Insert an element into the container                         |
| operator< and operator<=    | Returns boolean - compare contents of 2 containers           |
| operator> and operator>=    | Returns boolean - compare contents of 2 containers           |
| operator== and operator!=   | Returns boolean - are the contents of 2 containers equal or not |
| swap                        | Swap the elements of 2 containers                            |
| erase                       | Remove element(s) from a container                           |
| clear                       | Remove all elements from a container                         |
| begin and end               | Returns iterators to first element or end                    |
| rbegin and rend             | Returns reverse iterators to first element or end            |
| cbengin and cend            | Returns constant iterators to first element or end           |
| crbegin and crend           | Returns constant reverse iterators to first element or end   |

##### What Types of Elements Can Store in Containers?

+ A copy of the element will be stored in the container
+ All primitives OK
+ Element should be 
	+ Copyable and assignable (copy constructor / copy assignment)
	+ Movable for efficiency (move constructor / move assignment)
+ Ordered associative containers must be able to compare elements
	+ `operator<`, `operator==`
+ One exception: if we have raw pointer, then we cannot use Containers

### Algorithm

+ Algorithms - Functions for processing sequences of elements from Containers (around 60 algorithms)
	+ EX. `find`, `max`, `count`, `accumulate`, `sort`, etc

##### Types of Algorithms

+ Modifying - it modifies the sequence that it acts upon
+ Non-modifying - it does not modifies the sequence that it acts upon

### Iterators

+ Iterators - Generate sequences of element from containers
	+ EX. `forward`, `reverse`, `by value`, `by reference`, `constant`, etc
+ Goal of Iterator - Iterators allow us to think of a Container as a sequence of elements. It does not matter what the Container is. There very different Containers but iterators allow us to process sequence of elements from these containers without worrying or even needing to know about how the Container is implemented behind the scenes.

##### Types of Iterators

+ Input Iterators - it makes Container elements available to your program
+ Output Iterators - it can iterate over a sequence and write an element to a Container
+ Forward Iterators - it can iterate forward over a sequence and can rewrite any element
+ Bi-directional Iterators - like Forward Iterators, but they can iterate over a sequence in both directions
+ Random Access Iterators - it can use the subscript operator to directly access elements

##### How to Declare Iterators

```C++
std::vector<int>::iterator it1;
std::list<std::string>::iterator it2;
std::map<std::string, std::string>::iterator it3;
std::set<char>::iterator it4;
```
+ Use vector as example
```C++
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

##### What Operations We Can Do on Iterators?

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

##### Example of Iterator

+ Print at forward direction:
 ```C++
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
```C++
std::vector<int> vec {1,2,3};
 
std::vector<int>::reverse_iterator it = vec.begin(); // now it pointers to the end of vec
 
while (it != vec.end()) {
 	std::cout << *it << " ";
	++it;   // Actually move backward
}
// Output: 3, 2, 1
```

##### Other Iterators

+ `begin()` and `end()`
  + `iterator`
+ `cbegin()` and `cend()`
  + `const_iterator`
+ `rbegin()` and `rend()`
  + `reverse_iterator`
+ `crbegin()` and `crend()`
  + `const_reverse_iterator`

