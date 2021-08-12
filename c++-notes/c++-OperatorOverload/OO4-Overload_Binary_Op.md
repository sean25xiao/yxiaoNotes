# C++ Operator Overloading 4 - Overloading Binary Operator

## Overload Binary Operator (`+`, `-`, `==`, `!=`, `<`, `>`) by Member Function

+ `ReturnType Type::operatorOp(const &Type rhs);`
```C++
Number Number::operator+(const Number &rhs) const;
Number Number::operator-(const Number &rhs) const;   
bool Number::operator==(const Number &rhs) const;   
bool Number::operator<(const Number &rhs) const;

Number n1 {100}, n2 {200}; 
Number n3 = n1 + n2;              // n1.operator+(n2)
n3 = n1 - n2;                     // n1.operator-(n2)
if (n1==n2)                       // n1.operator==(n2)
```

+ Example 1: Implementing string compare method with overloaded `==`
```C++
bool operator==(const Mystring &rhs) const; // declare in class Mystring()

bool Mystring::operator==(const Mystring &rhs) const {
	if (std::strcmp(str, rhs.str) == 0)
		return true;
	else
		return false;
}

if (s1 == s2)       // s1 and s2 are Mystring objects
```

+ Example 2: Implement string add method with overloaded `+`
```C++
Mystring larry {"Larry"};
Mystring moe{"Moe"};
Mystring stooge {" is one of the three stooges"};

Mystring result = larry + stooges;    // larry.operator+(stooges)

result = moe + " is also a stooge"    // moe.operator+("is also a stooge")

result = "Moe" + stooges;             // "Moe".operator+(stooges)  **ERROR!!**
```

```C++
Mystring Mystring::operator+(const Mystring &rhs) const {
	size_t buff_size = std::strlen(str) + std::strlen(rhs.str) + 1;
	char *buff = new char[buff_size]
	std::strcpy(buff, str);
	std::strcat(buff, rhs.str);
	Mystring temp {buff};
	delete [] buff;
	return temp;
}
```

## Overload Binary Operator (`+`, `-`, `==`, `!=`, `<`, `>`) by Global Function

```C++
Number operator+(const Number &lhs, const Number &rhs);
Number operator-(const Number &lhs, const Number &rhs);   
bool operator==(const Number &lhs, const Number &rhs);   
bool operator<(const Number &lhs, const Number &rhs);

Number n1 {100}, n2 {200}; 
Number n3 = n1 + n2;              // operator+(n1, n2)
n3 = n1 - n2;                     // operator-(n1, n2)
if (n1==n2)                       // operator==(n1, n2)
```

+ Example 1: Implementing string compare method with overloaded `==`
```C++
// declare as freind in class Mystring
// Doesn't matter in private or public
friend bool operator==(const Mystring &lhs, const Mystring &rhs); 

bool operator==(const Mystring &lhs, const Mystring &rhs) {
	if (std::strcmp(lhs.str, rhs.str) == 0)
		return true;
	else
		return false;
}

// if declared as friend of Mystring, it can access private str attribute
// Otherwise, it must use getter methods
```

+ Example 2: Implement string add method with overloaded `+`
```C++
Mystring larry {"Larry"};
Mystring moe{"Moe"};
Mystring stooge {" is one of the three stooges"};

Mystring result = larry + stooges;    // operator+(larry, stooges)

result = moe + " is also a stooge"    // operator+(moe, "is also a stooge")

result = "Moe" + stooges;             // OK with non-member functions

result = "Moe" + " is also a stooge"; // error! two string won't work, C++ assumes primitive pointer, and not objects
```

```C++
Mystring operator+(const Mystring &lhs, const Mystring &rhs) {
	size_t buff_size = std::strlen(lhs.str) + std::strlen(rhs.str) + 1;
	char *buff = new char[buff_size]
	std::strcpy(buff, lhs.str);
	std::strcat(buff, rhs.str);
	Mystring temp {buff};
	delete [] buff;
	return temp;
}
```