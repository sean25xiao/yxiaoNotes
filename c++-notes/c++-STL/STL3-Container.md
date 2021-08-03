# C++ STL 3 - Container

## Types of Containers

+ **Sequence Containers** - the containers that maintain the ordering of inserted elements
  + Array, Vector, List, forward_list, Deque
+ **Associative Containers** - insert elements in a predefined order or no-order
  + `set`, `multi-set`, `map`, `multi-map`
+ **Container Adapters** - do not support iterators, so they do not use with STL algorithms
  + Stack, Queue, Priority queue

## Common Container Member Function

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

## What Types of Elements Can Store in Containers?

+ A copy of the element will be stored in the container
+ All primitives OK
+ Element should be 
  + Copyable and assignable (copy constructor / copy assignment)
  + Movable for efficiency (move constructor / move assignment)
+ Ordered associative containers must be able to compare elements
  + `operator<`, `operator==`
+ One exception: if we have raw pointer, then we cannot use Containers