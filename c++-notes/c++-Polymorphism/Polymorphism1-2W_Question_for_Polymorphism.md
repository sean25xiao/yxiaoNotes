# C++ Polymorphism 1 - What is and Why need Polymorphism 

##  What is Polymorphism?

+ Two kinds of polymorphism we have seen
	+ Overloaded function
	+ Overloaded operators
+ Different Type of Polymorphisms
	+ Compile Time Polymorphism / Static Binding / Early Binding
		+ Overloaded Functions
		+ Overloaded Operators
		+ It means before the program executes
	+ Runtime Polymorphism / Dynamic Binding / Late Binding
		+ Function Overriding
			+ Using Virtual Functions
			+ Using Base Class Pointers and References

## Why is Polymorphism?

### Example 1: Problem in using a Pointer for Derived Object

```C++
/*
        Account
		   |
		   |
		 --------
		 |       |
		 |       |
	  Savings  Checking
		 |
		 |
	   Trust
	   
	** All class has a mthod called withdraw()
*/

Account a;         
a.withdraw(1000);   // Account::withdraw()

Savings b;         
b.withdraw(1000);   // Savings::withdraw()

Checking c;       
c.withdraw(1000);   // Checking::withdraw()

Trust d;           
d.withdraw(1000);   // Trust::withdraw()

Account *p = new Trust();
P->widthdraw(1000);    // Account::withdraw()
                       // !!!! but should be
					   // Trust::widthdraw()
```

+ For example, we call `withdraw()` method in `a`, the compiler knows this method and binds this method call to `Account` at  compile time or statically. Same as `b`, `c`, `d`
+ However, if we dynamically create a `Trust` object in heap, `p` is the pointer points to an account object. **But we use static binding as default so the compiler does not really know what type of account object is pointing to at runtime.** All it knows is that `p` is pointing to an account so it will call the `Account` withdraw method which is WRONG!
+ **How to Solve It?**: make `withdraw()` method a `virtual` function in `Account` class
	+ The idea is: **using virtual function to tell the compiler not to bind the call in compile time, but instead, defer the binding to runtime and use a check to see exactly what P is pointing to and then that object's method will be called**

### Example 2: Problem in Passing a Derived Object to a Function

```C++
void display_account (const Account &acc) {
	acc.display();  // will always use Account::display which is WRONG!!!!
}

Account a;
display_account(a);

Savings b;
display_account(b);

Checking c;
display_account(c);

Trust d;
display_account(d);
```

+ Compiler does not care what types of account object it passed in the `display_account()`  function, so it will always use `Account` class method
+ **How to Solve It?**: make `display_account()` method a virtual function in `Account` class