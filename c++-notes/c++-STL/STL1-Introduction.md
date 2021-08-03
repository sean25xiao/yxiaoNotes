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

