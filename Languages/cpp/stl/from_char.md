# from_chars

C++17 引入，头文件 `#include <charconv>`

```cpp
std::from_chars_result from_chars( const char* first, const char* last,
                                   T& value, int base = 10 );
std::from_chars_result from_chars( const char* first, const char* last, 
                                   double& value,
                                   std::chars_format fmt std::chars_format::general );
std::from_chars_result from_chars( const char* first, const char* last,
                                   long double& value,
                                   std::chars_format fmt = std::chars_format::general );
struct from_chars_result {
    const char* ptr;  // [first, last) 中第一个未匹配字符
    std::errc ec;    // 失败原因
};
```
