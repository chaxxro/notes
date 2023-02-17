# noexcept

## 说明符
`noexcept` ：对外宣称处理不会抛出异常
```cpp
void fun() noexcept
{

}
```
一旦声明某个处理是 `noexcept`，编译器在处理这部分代码的时候似乎也会省略错误传递的那部分功能

## 运算符

`noexcept` 运算符针对某个表达式执行编译期检测，如果该表达式被声明为不会抛出异常，则 `noexcept` 运算符返回 `true`，否则返回 `false`

```cpp
void MyFunc() noexcept {

}

bool isNoexcept = noexcept(MyFunc());  // true
```

`noexcept` 运算符不会对表达式求值，仅仅只是检测表达式是否被声明为不会抛出异常