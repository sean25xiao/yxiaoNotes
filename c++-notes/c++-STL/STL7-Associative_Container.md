# C++ STL 7 - Associative Containers

## `std::set`

+ Similar to mathematical set
+ Ordered by key
  + `std::unordered_set` allows unordered elements
+ No duplicated elements
  + `std::multi_set` allows duplicated elements
+ No concept of front and back
+ `std::unordered_multiset` allows duplicated and unordered elements 
+ `#include <set>`

```c++
std::set<int> s {1,2,3,4,5};

std::set<std::string> stooges {
	std::string{"Larry"},
	"Moe",
	std::string{"Curly"}
};

s = {2,4,6,8,10};
```

```c++
std::set<int> s {4,1,1,3,3,2,5};  // 1 2 3 4 5, uses operator< for ordering

std::cout << s.size();    // 5
std::cout << s.max_size;   // a very large number

s.insert(7);     // 1 2 3 4 5 7

Person p1 {"Larry", 18};
Person p2 {"Yibang", 25};
    
std::set<Person> stooges;

stooges.insert(p1);                // add p1 to the set
auto result = stooges.insert(p2);  // add p2 to the set
    // returns a std::pair<iterator, bool>
       // first is an iterator to the inserted element 
       // second is a boolean indicating success or failure
```

```c++
std::set<int> s {1,2,3,4,5};

s.erase(3);   // erase the 3:  1 2 4 5

auto it = s.find(5);
if (it != s.end())
	s.erase(it);    // erase the 5:  1 2 4

int num = s.count(1);  // 0 or 1, this can know whether an element is in the set or not

s.clear();   // remove all elements

s.empty();   // true or false
```

## `std::map`

+ Similar to dictionary
+ Elements are stored as <Key, Value> pairs (`std::pair`)
+ Ordered by key
  + `std::unordered_map` allows unordered elements
  + Need `#include <unordered_map>`
+ No duplicate elements (keys are unique)
  + `std::multi_map` allows duplicated elements
+ Direct element access using the key
+ `std::unordered_multimap` allows duplicated and unordered elements
  + Need `#include <unordered_map>`
+ `#include <map>`

```c++
std::map<std::string, int> m1 {
	{"Larry", 18},
	{"Moe", 25}
};

std::map<std::string, std::string> m2 {
	{"Yibang", "Anqi"},
	{"GeWang", "FengXiao"},
	{"BB", "PP"}
};

std::cout << m2.size();   // 3

// How to insert in Map
std::pair<std::string, std::string> p1 {"James", "Mechanic"};

m2.insert(p1);
// or 
m2.insert(std::make_pair("Roger", "Ranger"));

m2["Frank"] = "Teacher";       // insert
m2["Frank"] = "Instructor";    // update value
m2.at("Frank") = "Professor";  // update value
```

```c++
std::map<std::string, std::string> m {
	{"Bob", "Butcher"},
	{"Anne", "Baker"},
	{"George", "Maker"}
};

m.erase("Anne");    // erase Anne

if (m.find("Bob") != m.end())
	std::cout << "Found Bob!";
	
auto it = m.find("George");
if (it != m.end())
	m.erase(it);      // erase George

int num = m.count("Bob");   // 0 or 1

m.clear();     // remove all elements
 
m.empty();     // true or false
```

