# move

`#include <utility>`

`std::move()` 执行到右值的无条件转换，返回右值引用本质上还是靠 `static_cast<T&&>` 完成的

```cpp
template<typename T>
typename remove_reference<T>::type && move(T&& t)
{
    return static_cast<typename remove_reference<T>::type &&>(t);
}

std::move(std::string("111"))
// T 是 std::string
// 返回 std::string&& 形参 std::string&&

std::string s1("111")
std::move(s1)
// T 是 std::string&
// 返回 std::string&& 形参 std::string&&
```

`std::move` 将左值转换成右值，从而可以方便使用移动语义

对于自定义类型，真正使用移动语义还需手动实现移动构造和移动赋值

- 如果没有提供移动构造函数，只提供了拷贝构造函数，`std::move()` 会失效然后

- c++11 中的所有容器都实现了 `move` 语义

- 如果是一些基本类型，如 `int` 和 `char[10]` 数组等，如果使用 `move` 仍会发生拷贝
