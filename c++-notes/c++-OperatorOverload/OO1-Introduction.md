# C++ Operator Overloading 1 - Introduction

## Why need Operator Overloading

+ Ex. If we have a Number Class that models any number: `(a + b) * (c / d)`
+ By using functions: `Number result = multiply(add(a, b), divide(c, d));`
+ By using member methods: `Number result = (a.add(b)).multiply(c.divide(d));`
+ Hard to read and write code
+ Why don't use `+, -, *, /` etc. since we have used in school?
+ If we use operator overloading: Number result = `(a+b)*(c/d)`

## What Operators can be Overloaded

+ C++ allows user to overload most of the operator
+ There are some operators that **cannot** be overloaded:
	+ `::`
	+  `:?`
	+ `.*`
	+ `.`
	+ `sizeof`
	+ `int`
	+ `double`
	+ Cannot create new operators

## Example We Will used in Operator Overloading Study

+ A Class that tries to simulate Strings

```C++
// This is the example of class we are going to use in this example
// Mystring.h
#ifndef _MYSTRING_H_
#define _MYSTRING_H_

class Mystring {

private:
    char *str; // C-style string  
public:
    Mystring();                       // No-args constructor
    Mystring(const char *s)           // Overloaded constructor
    Mystring(const Mystring &source); // Copy Constructor
    ~Mystring();
    void display() const;
    int get_length() const;           // getters
    const char *get_str() const;
}

#endif // _MYSTRING_H_
```

```C++
// Mystring.cpp
#include <cstring>
#include <iostream>
#include "Mystring.h" 

// No-args constructor
Mystring::Mystring()
   : str{nullptr} {   // create a null pointer
   str = new char[1]  // allocate one char in heap, and assign str pointer to point it
   *str = '\0'        // dereference the pointer and put termintor in the char
 } 
 
 // Overloaded Constructor
 Mystring::Mystring(const char *s)    // if input is "Hello"
   : str{nullptr} {
   if (s == nullptr) {
      str = new char[1]
      *str = '\0'
   } else {
      str = new char[std::strlen(s)+1];
      std::strcpy(str, s)   // copy s to str, ex: "Hello\0"
   }  
 } 
 
 // Copy Constructor
 Mystring::Mystring(const Mystring &source)
   : str{nullptr} {
   str = new char[std::strlen(source.str) + 1];
   std::strcpy(str, source.str);
 } 
 
 // Destructor
 Mystring::~Mystring() {
   delete [] str;
 } 
 
 // Display method
 void Mystring::display() const {
   std::cout << str << ":" << get_length() << std::endl
 } 
 
 // length getter
 int Mystring::get_length() const { return std::strlen(str); } 
 
 // string getter
 const char *Mystring::get_str() const { return str; }
```

```C++
// main.cpp

#include <iostream>
#include "Mystring.h"

using namespace std;

int main() {
   Mystring empty;            // no-args constructors
   Mystring larry("Larry");   // overloaded constructors
   Mystring stooge {larry};   // copy constructors
   
   empty.display();    
   larry.display();    
   stooge.display();  
   
   return 0;
} 
```