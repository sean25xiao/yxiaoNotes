# C++ Operator Overloading 2 - Overload `=`

## Overloaded `=` by using Shallow Copy

+ C++ provides a **default assignment operator** used for assigning one object to another
+ It shares the same problem of Shallow Copy Problem

```C++
Mystring s1 {"Frank"};
Mystring s2 = s1;  // It is NOT assignment
                   // It is overloaded by Copy Constructor (default as shallow copy!)
				   // Same as Mystring s2{s1}
				   
s2 = s1;           // This is assignment, because s2 has been created and initialized
```

## Overloaded `=` by using Deep Copy

+ Template for overloading `=` by using Deep Copy
```C++
Type &Type::operator=(const Type &rhs);

Mystring &Mystring::operator=(const Mystring &rhs);

s2 = s1             // we write this

s2.operator=(s1);   // same as this: operator= method is called
```

+ Implement the Overloading Methods for `=` in the String Example by using Deep Copy
```C++
Mystring &operator=(const Mystring &rhs)  // declare in class Mystring

Mystring &Mystring::operator=(const Mystring &rhs) { 
   if (this == &rhs)                            // if s1=s1
       return *this;                            // do nothing, just return current object address
	   
   delete [] str;                               // deallocate everything that was on the heap
   str = new char[std::strlen(rhs.str) + 1];    // allocate new space
   std::strcpy(str, rhs.str);                   // copy the string
   
   return *this;
}
```

+ In `main()`, we can directly use `=` for two String Objects
```C++
int main() {
   Mystring a {"Hello"};  // overloaded constructor
   Mystring b;            // no-args constructor
   b = a;                 // copy assignment (already implemented)
   
   b = "This is a test";  // b.operator=("This is a test")
                          // It will create a object to assign string "This is a test" in it
						  // then call the overloaded copy assignment method to deep copy to Object b
}
```

## Overloaded `=` by using Move Constructor

+ Overloaded `=` can use temporary object at the right side
```C++
Mystring s1;

s1 = Mystring {"Frank"};  // Move assignment
                          // Because RHS is an r-value
						  // Why it is an r-value: it has no name, it is a temporary object
```

```C++
Type &Type::operator=(Type &&rhs)

Mystring &Mystring::operator=(Mystring &&rhs);

s1 = Mystring{"Joe"};  // move operator= called

s1 = "Frank";          // move operator= called
```

+ Difference between overload `=` by Deep Copy
	+ Similar with the difference between Move Constructor and Deep Copy in OOP Section
	+ It uses r-value as input argument
	+ It does not use `const` because we need to modify the temporary object

+ How we implement the move assignment operator overloading method?
```C++
Mystring &Mystring::operator=(Mystring &&rhs) {
	if (this == &rhs)
		return *this;
		
	delete [] str;      // deallocate the current storage
	str = rhs.str;      // steal the pointer from rhs
	
	rhs.str = nullptr;  // null out the rhs object
	
	return *this;       // return current object
}
```
+ Example:
```C++
int main () {
	Mystring a {"Hello"};  // Overloaded Constructor
	a = Mystring {"Hola"}; // Overloaded constructor then move assignment, **unnamed temp object**
	b = "Bonjour";         // Overloaded constructor then move assignment
	                       // Note: if we don't declare move assignment overloading,
						   // **it will use defualt copy assignment**
}
```