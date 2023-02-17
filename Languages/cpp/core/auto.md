# auto

`auto` 在声明变量的时候根据变量初始值的类型自动为此变量选择匹配的类型

- 属于编译期的类型推导

- 必须在定义时初始化

- 如果初始化表达式是引用，则去除引用语义

```cpp
int a = 10;
int &b = a;

auto c = b;// c 的类型为 int 而非 int&（去除引用）
auto &d = b;  // 此时 c 的类型才为 int&
```

- 如果初始化表达式为 `const` 或 `volatile`，则除去 `const` 或 `volatile` 语义

```cpp
const int a1 = 10;
auto  b1= a1;  // b1 的类型为 int 而非 const int（去除 const）
const auto c1 = a1;  // 此时 c1 的类型为 const int
b1 = 100;//合法
c1 = 100;//非法
```

- 如果 `auto` 关键字带上 `&` 或 `*` 时，则不去除 `const` 或 `volatile` 语意

```cpp
const int a2 = 10;
auto &b2 = a2;  //因为 auto 带上 &，故不去除 const，b2 类型为 const int &
b2 = 10; //非法
```
- 初始化表达式为数组时，`auto` 关键字推导类型为指针；若表达式为数组且 `auto` 带上 `&`，则推导类型为数组类型

- 函数或者模板参数不能被声明为 `auto`