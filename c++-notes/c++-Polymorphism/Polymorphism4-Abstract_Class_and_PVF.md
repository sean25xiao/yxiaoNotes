# C++ Polymorphism 4 - Abstract Class and Pure Virtual Functions

## Abstract Class and PVF

+ **Abstract Class**
  + 这种 Class 不能用它来创建 object，会造成 Compiler Error
  + 通常 Abstract Class 用来在 Inheritance Hierarchies 里面当作 Base Class，叫做 **Abstract Base** Classes
  + 通常不提供 implementation
+ **Concrete Class**
  + 可以用来创建 object，之前学习的 class 都是 Concrete Class
  + 它的所有的 method 都被定义了，提供 implementation
    + `Checking Account`，`Savings Account`
    + `Faculty`，`Staff`
+ **Abstract Base Class**
  + 属于非常通用的 Base Class，通常不能直接从这个 Base Class 来创建 object
  + 比如：一个人如果要开 Account，必须要指明 Account 类型，比如 Saving 还是 Checking，不能只开 Account
  + 这种类型的 Class 是用于为其他 Derived Class 提供 Base Class 的基本功能
  + 必须至少有一个 Pure Virtual Function
  + Pure Virtual Function
    + 主要作用是让整个 class 变成 Abstract Base Class
    + 只需要在 virtual method 后面加上 `=0` 即可
    + `virtual void function() = 0;  // pure virtual function`
    + 一般不用 implementation
  + Derived Class 必须 override base class
    + 如果不 override 的话，那么这个 Derived Class 也将变成 Abstract Class

```c++
// Section 16
// Pure virtual functions and abstract base classes
#include <iostream>
#include <vector>

class Shape {			// Abstract Base class
private:
       // attributes common to all shapes
public:
      virtual void draw() = 0;	 // pure virtual function
      virtual void rotate() = 0;   // pure virtual function
      virtual ~Shape() { }
};

class Open_Shape: public Shape {    // Abstract class since no override
public:
    virtual ~Open_Shape() { }
};

class Closed_Shape : public Shape {  // Abstract class since no override
public:
    virtual ~Closed_Shape() { };
};

class Line: public Open_Shape {     // Concrete class
public:
    virtual void draw() override {
        std::cout << "Drawing a line" << std::endl;
    }
    virtual void rotate() override {
        std::cout << "Rotating a line" << std::endl;
    }
    virtual ~Line() {}
};

class Circle: public Closed_Shape {     // Concrete class
public:
    virtual void draw() override {
        std::cout << "Drawing a circle" << std::endl;
    }
    virtual void rotate() override {
        std::cout << "Rotating a circle" << std::endl;
    }
    virtual ~Circle() {}
};

class Square: public Closed_Shape {     // Concrete class
public:
    virtual void draw() override {
        std::cout << "Drawing a square" << std::endl;
    }
    virtual void rotate() override {
        std::cout << "Rotating a square" << std::endl;
    }
    virtual ~Square() {}
};


void screen_refresh(const std::vector<Shape *> &shapes) {
    std::cout << "Refreshing" << std::endl;
    for (const auto p: shapes) 
        p->draw();
}

int main() {
//    Shape s;                    // Give compiler error
//    Shape *p = new Shape();     // Give compiler error

//        Circle c;
//        c.draw();

//    Shape *ptr = new Circle();
//    ptr->draw();
//    ptr->rotate();

    Shape *s1 = new Circle();
    Shape *s2 = new Line();
    Shape *s3 = new Square();
    
    std::vector<Shape *> shapes {s1,s2,s3};
    
//    for (const auto p: shapes) 
//        p->draw();
        
    screen_refresh(shapes);
    
    delete s1;
    delete s2;
    delete s3;
    
    return 0;
}
```

## Use Abstract Class as Interface

+ Abstract Base Class 可以被用作 Interface
  + C++ 不像 Java 或者 C# 有 Interface 定义，但我们可以用 Abstract Base Class + PVF 来自己做一个 Interface
  + 通常可以以 I_ 开头：I_Shape, I_Account

```c++
class Printable {  // Printable class used as interface
	friend ostream &operator<<(ostream &, const Printable &obj);
public:
	virtual void print(ostream &os) const = 0;
	virtual ~Printable() {};
};

ostream &operator<<(ostream &os, const Printable &obj) {
	obj.print(os)
	return os;
}

class Any_Class : public Printable {
public:
    // must override Printable::print()
    virtual void print(ostream &os) override {
        os << "Hi from Any_Class";
    }
    ......
}

void function1 (Any_Class &obj) {
    cout << obj << endl;
}

int main() {
    Any_Class << *ptr << endl;
    cout << *ptr << endl;
    
    function1(*ptr);  // "Hi from Any_Class"
}
```



