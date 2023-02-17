# string_view

一种同时拥有 C 语言的 `const char*` 的指针拷贝成本和 C++ 语言中 `std::string` 类大部分 api 的类型

作为函数参数时只使用值拷贝形式 `std::string_view`，不要引用字符串视图 `std::string_view&`

本质上是持有一个字符串的指针，因此需要保证被持有的字符串生命周期比 `std::string_view` 长，被持有的字符串在 `std::string_view` 变量生命周期结束之前保持不变

可能传入只读 C 风格字符串参数时，优先使用 `std::string_view`

```cpp
void StringDisplay(const std::string& str) {
    std::cout << "const std::string& 版本：" << str << std::endl;
}

// 传入一个字符串进行打印
int main() {
    // case 1:
    StringDisplay("打印一个字符串");  // 此时触发了一次字符串拷贝
    // case 2: 
    const char* str = "C风格字符串";
    StringDisplay(str);  // 此时触发了一次字符串拷贝    
}
// 使用 string_view 则不会拷贝
```

不拥有 `c_str()` 方法

```cpp
void Display(const char* str) {
    std::cout << str << "\n";
}

void StringViewDefect(){
    std::string_view sv_str("ABC",1); //新建一个长度为1的string_view对象
    std::cout << sv_str << "\n";  // 输出 “A”
   
    //需要调用入参为C风格字符串的函数
    DisPlay(sv_str.data()); // 此时输出“ABC”，因为经过处理的字符串视图无法完美转成C风格字符串
}
```