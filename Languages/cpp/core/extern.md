# extern

`extern` 是全局的意思

## 声明和定义

- 变量的定义：用于为变量分配存储空间，还可以为变量指定初始值

- 变量的声明：用于向程序表明变量的类型和名字；定义也是声明，可以通过使用 `extern` 关键字声明变量名而不定义它

```cpp
extern int i; //声明一个变量i，但是并没有定义
int i; //声明并且定义了一个变量i
extern int j = 0;  // 定义了一个变量
```

`extern` 声明不是定义，也不分配存储空间，它只是说明变量定义在程序的其他地方

只有当声明也是定义时，声明才可以有初始化式，因为只有定义才分配存储空间

如果声明有初始化式，那么它可被当作是定义，即使声明标记为 `extern`，如 `extern int i = 0`

## 声明 C 函数

C++ 虽然兼容 C，但 C++ 文件中函数编译因为C++支持函数重载，所以生成的符号与 C 语言生成的不同

`extern "C"` 告诉编译器在编译函数名时按 C 的规则去翻译相应的函数名而不是 C++ 的，C++ 的规则在翻译这个函数名时会把名字变得面目全非

`extern "C"` 需要放在 C++ 头文件中
