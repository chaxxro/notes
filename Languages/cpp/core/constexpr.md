# constexpr

`constexpr`：可以用来修饰变量、函数、构造函数

任何元素被 `constexpr` 修饰，那么编译器将之看成编译时就能得出常量值的表达式去优化

- 变量定义，变量会隐式包含 `const`

```cpp
constexpr int value = 0;
```

- 函数或模板函数

```cpp
constexpr int func() {
    return 0;
}
```

- 静态数据

```cpp
static constexpr char name[] = "jj"
```

