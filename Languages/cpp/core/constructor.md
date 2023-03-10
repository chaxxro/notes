# 构造函数

因为类的部分数据的访问状态是私有的，意味着不能像结构一样初始化，所以需要构造函数来初始化类实例对象

## 使用构造函数

- 显式调用构造函数： `Stock food = Stock("xxxx", 100);  // 与构造函数形参列表对应`

- 隐式调用构造函数： `Stock food("xxxx", 100);  // 与构造函数形参列表对应`

## 默认构造函数 

默认构造函数是在未提供显式初始值时，用来创建对象的构造函数 `A a`，默认构造函数没有参数，如果类没有提供任何构造函数（当且只当），C++ 将自动提供默认的构造函数，它不做任何工作

## 复制构造函数 

当使用一个对象来初始化另一个对象时，编译器将调用复制构造函数

复制构造函数用于将一个对象复制到新创建的对象中，它用于初始化过程中（包括按值传递参数），而不是常规的赋值过程原型： `Class_name(const Class_name &)`

- 新建一个对象并将其初始化为同类现有对象时，复制构造函数被调用；最常见的情况：将新对象显式地初始化为现有的对象

- 程序生成对象副本时，编译器都将使用复制构造函数；最常见的情况：当函数按值传递对象或函数返回对象时，都将使用复制构造函数

- 默认的复制构造函数逐个复制非静态成员（浅复制），复制的是成员的值

- 如果类中包含了使用new初始化的指针成员，应当定义一个复制构造函数，以深复制数据

```cpp
// 调用复制构造函数
StringBad ditto(motto);
StringBad metoo = motto;
StringBad also = StringBad(metto);
StringBad * pStringBad = new StringBad(motto);

// 调用自动为类生成的默认重载赋值运算符
// 原型：Class_name & Class_name::operator=(const Class_name &)
String knot;
knot = metto;
```

## 移动构造函数

```cpp
MyClass(MyClass &&)
```

## 委托构造函数

C++11 允许在一个构造函数定义中使用另一个构造函数

## 构造函数初始化过程

构造函数的执行可以分成两个阶段，初始化阶段和计算阶段，初始化阶段先于计算阶段

所有类成员都会在初始化阶段初始化，即使该成员没有出现在构造函数的初始化列表中

在初始化列表中构造成员类，可以省去调用默认构造函数的过程，直接调用指定构造函数进行初始化

初始化列表中的成员是按照在类中出现的顺序进行初始化的，而不是按照在初始化列表出现的顺序初始化

```cpp
struct Test1 {
    Test1() { 
        cout << "Construct Test1" << endl ;
    }

    Test1(const Test1& t1) {
        cout << "Copy constructor for Test1" << endl ;
        this->a = t1.a ;
    }

    Test1& operator = (const Test1& t1) {
        cout << "Assignment for Test1" << endl ;
        this->a = t1.a ;
        return *this;
    }

    int a ;
};

struct Test2 {
    Test1 test1 ;
    Test2(Test1 &t1) {
        test1 = t1 ;
    }
};

Test1 t1;
Test2 t2(t1);

/*
Construct Test1
Construct Test1
Assignment for Test1
*/
```

## 注意事项

- 当只定义了移动构造时，拷贝构造、拷贝赋值和移动赋值都不自动生成

- 当只定义了移动赋值时，拷贝构造、拷贝赋值和移动构造都不自动生成

- 当只定义了拷贝构造时，会生成拷贝赋值，移动操作会转而执行拷贝操作

- 当只定义了拷贝赋值时，会生成拷贝构造，移动操作会转而执行拷贝操作

```cpp
class Demo {
public:
  Demo() { std::cout << "In default constructor\n"; }
  ~Demo() { std::cout << "In destructor\n"; }

  Demo(const Demo& d) { std::cout << "In copy constructor\n"; }
  Demo(Demo&& d) { std::cout << "In move constructor\n"; }

  Demo& operator=(const Demo& d) { std::cout << "In copy assign\n"; }
  Demo& operator=(Demo&& d) { std::cout << "In move assign\n"; }
};

int main() {
  Demo demo;

  Demo d0(demo);

  Demo d1;
  d1 = demo;

  Demo d2(std::move(demo));

  Demo d3;
  d3 = std::move(demo);
}
```