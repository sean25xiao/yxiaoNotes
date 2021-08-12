# C++ Operator Overloading 5 - Overload Stream Insertion and Extraction Op (`<<`, `>>`)

```C++
Mystring larry {"Larry"};

cout << larry << endl;  // Larry

Player hero {"Hero", 100, 33};

cout << hero << endl;  // {name: Hero, health: 100, xp:33}

//=======================

Mystring larry;

cin >> larry;

Player hero;

cin >> hero;
```

+ Don't make sense to implement this overloading by Member Function
	+ Left operand must be a user-defined class
	+ Ex. `larry << cout`, or `hero >> cin`, it is very wired
	+ So we have to overload these operators as non-member functions (global functions)
+ It is in `std::`, so it is a **non-member method function**
	+ It should be the Friend of the Class

## Overload Stream Insertion Operator `<<`

```C++
// First arg is an output stream object by reference
// Second arg is reference to the Mystring object

std::ostream &operator<<(std::ostream &os, const Mystring &obj) {
	os << obj.str;           // if friend function
	// os << obj.get_str();  // if not friend function
	return os;     // return a reference to the ostream so we can keep inserting
	               // Don't return ostream by value!!
}
```

## Overload Stream Extraction Operator `>>`

```C++
// The second arg is not const, becuase we need to modify th string inside

std::istream &operator>>(std::istream &is, Mystring &obj) {
	char *buff = new char[1000];
	is >> buff;
	obj = Mystring{buff};  // If have copy or move assignment
	delete [] buff;
	return is;
}
```

## A Complete Example of Overloading Stream Operators

```C++
//in Mystring.h

class Mystring
{
	friend std::ostream &operator<<(std::ostream &os, const Mystring &rhs);
	friend std::ostream &operator>>(std::istream &is, Mystring &rhs);
private:
	char *string;    // pointer to a char[] that holds a C-style string
public:
	Mystring();                          // No-args Constructor
	Mystring(const char *s);             // Overloaded Constructor
	Mystring(const Mystring &source);    // Copy constructor
	Mystring(Mystring &&source);         // Move Constructor
	~Mystring();                         // Destructor
	......
}
```

```C++
// in Mystring.cpp

std::ostream &operator<<(std::ostream &os, const Mystring &rhs) {
	os << rhs.str;       
	return os;     
}

std::istream &operator>>(std::istream &in, Mystring &rhs) {
	char *buff = new char[1000];
	in >> buff;
	rhs = Mystring{buff};  // If have copy or move assignment
	delete [] buff;
	return in;
}
```