# C++ STL 5 - Algorithm

+ 很多算法需要额外的信息来完成它们的工作
  + Functors (Function Objects)
  + Function Pointers
  + Lambda Expressions (C++11)
+ 不同的 Container 支持不同的 Iterator，而不同的 Iterator 支持不同的 Algorithm

## Example Algorithm

#### `Find`

+ find algorithm 可以定位要寻找的目标（第一次出现的时候）
+ 返回 an iterator pointing to the located element, or `end()`
+ 有很多变体

```c++
std::vector<int> vec {1,2,3};

auto loc = std::find(vec.begin(), vec.end(), 3);

if (loc != vec.end())                // found it!
	std::cout << *loc << std::endl;  // 3
```

#### `for_each`

+ 可以 apply a function to each element in the iterator sequence
+ 这个 function 必须按照以下格式
  + Functor (Function Objects)
  + Function Pointers
  + Lambda Expressions

**`for_each` using a functor**

```c++
struct Square_Functor {   // can also be Class with single method
	void operator() (int x) {
		std::cout << x * x << " ";
	}
};

Square_Functor square;    // Function object

std::vector<int> vec {1, 2, 3, 4};

std::for_each(vec.begin(), vec.end(), square);

// 1 4 9 16
```

**`for_each` using a function pointer**

```c++
void square(int x) {    // function
    std::cout << x * x << " ";
}

std::vector<int> vec {1, 2, 3, 4};

std::for_each(vec.begin(), vec.end(), square);  // not square(), it will call the function, but we want the function pointer

// 1 4 9 16
```

**`for_each` using a lambda express**

```c++
std::vector<int> vec {1, 2, 3, 4};

std::for_each(vec.begin(), vec.end(),
	[](int x) { std::cout << x * x << " "; })  // lambda
	
// 1 4 9 16
```

