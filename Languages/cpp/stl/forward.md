# forward

`std::move` 和 `std::forward` 都是编译期做类型转换

右值引用类型是独立于值的，一个右值引用参数作为函数的形参，在函数内部再转发该参数的时候它已经变成一个左值，并不是他原来的类型；需要一种方法能够按照参数原来的类型转发到另一个函数，这种转发类型称为完美转发

```cpp
template< class T >
T&& forward( typename std::remove_reference<T>::type& t ) noexcept;
```

`std::forward()` 实现了参数在传递过程中保持其值属性的功能，即若是左值，则传递之后仍然是左值，若是右值，则传递之后仍然是右值，返回值为 `static_cast<T&&>(t)`

`std::move` 执行到右值的无条件转换，`std::forward` 只有在它的参数绑定到一个右值上的时候才转换它的参数到一个右值

`std::forward` 不仅可以保持左值或者右值不变，同时还可以保持 `const`、`Lreference`、`Rreference`、`volatile` 等属性不变

```cpp
void PrintV(int &t) {
    cout << "lvalue" << endl;
}

void PrintV(int &&t) {
    cout << "rvalue" << endl;
}

template<typename T>
void Test(T &&t) {
    PrintV(t);
    PrintV(std::forward<T>(t));

    PrintV(std::move(t));
}

int main() {
    Test(1); // lvalue rvalue rvalue
    int a = 1;
    Test(a); // lvalue lvalue rvalue

    // std::forward 返回 T&&
    Test(std::forward<int>(a)); // lvalue rvalue rvalue
    Test(std::forward<int&>(a)); // lvalue lvalue rvalue
    Test(std::forward<int&&>(a)); // lvalue rvalue rvalue
    return 0;
}
```
