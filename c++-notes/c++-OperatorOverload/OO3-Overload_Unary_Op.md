# C++ Operator Overloading 3 - Overloading Unary Operator

## Overload Unary Operator (`++`, `--`, `-`, `!`) by Member Function

+ `ReturnType Type::operatorOp()`
```C++
Number Number::operator-() const;
Number Number::operator++();      // pre-increment
Number Number::operator++(int);   // post-increment
bool Number::operator!() const;

Number n1 {100}; 
Number n2 = -n1;                  // n1.operator-()
n2 = ++n1;                        // n1.operator++()
n2 = n1++                         // n1.operator++(int)
```

+ Example: Overload `-` to make lowercase
```C++
Mystring larry1 {"LARRY"};
Mystring larry2;

larry1.display();    // LARRY

larry2 = -larry1;    // larry1.operator-()

larry1.display();    // LARRY
larry2.display();    // larry

// How to implement overloaded operation method?
```

```C++
Mystring operator-() const;  // declare in class Mystring()

Mystring Mystring::operator-() const {
	char *buff = new char[std::strlen(str) + 1];
	std::strcpy(buff, str);
	for (size_t i=0; i<std::strlen(buff); i++)
		buff[i] = std::tolower(buff[i]);
	Mystring temp {buff};
	delete [] buff
	return temp;
}
```

## Overload Unary Operator (`++`, `--`, `-`, `!`) by Global Function

+ `ReturnType operatorOp(Type &obj);`
```C++
Number operator-(const Number &obj);
Number operator++(Number &obj);         // pre-increment
Number operator++(Number &obj, int);    // post-increment
bool operator!(const Number &obj);

Number n1 {100};
Number n2 = -n1;                        // operator-(n1)
n2 = ++n1;                              // operator++(n1)
n2 = n1++;                              // operator++(n1, int)
```

+ Example: Overload `-` to make lowercase
```C++
Mystring larry1 {"LARRY"};
Mystring larry2;

larry1.display();    // LARRY

larry2 = -larry1;    // larry1.operator-()

larry1.display();    // LARRY
larry2.display();    // larry

// How to implement overloaded operation **global function**?
```

```C++
// Often declared as friend functions in the class declaration

Mystring operator-(const Mystring &obj) {
	char *buff = new char[std::strlen(obj.str) + 1];   // str -> obj.str, since it is no longer a method any more
	std::strcpy(buff, obj.str);      // str -> obj.str
	for (size_t i=0; i<std::strlen(buff); i++)
		buff[i] = std::tolower(buff[i]);
	Mystring temp {buff};
	delete [] buff
	return temp;
}
```