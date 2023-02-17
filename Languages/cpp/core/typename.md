# typename

`typename` 用于引入一个模板参数，这个关键字用于指出模板中的非独立名称是类型名，而非变量名

```cpp
template <typename T>
const T& max(const T& x, const T& y);

template <class T>
void foo() {
    T::iterator * iter;
    // ...
}
/*
T::iterator实际上可以是以下三种中的任何一种类型：
1. 静态数据成员
2. 静态成员函数
3. 嵌套类型
*/

template <class T>
void foo() {
    typename T::iterator * iter;  // 表示 T::iterator 一定要是个类型才行
    // ... 
}

template <class T,class Alloc=alloc>
class vector{
public:
    //...
    typedef size_t size_type;
    //...
};
typedef typename std::vector<T>::size_type size_type;
```

`typedef` 创建了存在类型的别名，而 `typename` 告诉编译器是一个类型而不是一个成员

```cpp
// 模版实现以下代码
int result = 0;
while (n != 0) {
  result = result + n;
  n = n - 1;
}

template <bool condition,typename Body>
struct WhileLoop;

template <typename Body>
struct WhileLoop<true, Body> {
    typedef typename WhileLoop<Body::cond_value, typename Body::next_type>::type type;
};

template <typename Body>
struct WhileLoop<false, Body> {
    typedef typename Body::res_type type;
};
/*
Body 必须提供一个静态数据成员，cond_value
Body 必须提供两个子类型定义 res_type 和 next_type
*/

template <typename Body>
struct While {
    typedef typename WhileLoop< Body::cond_value, Body>::type  type;
};

/*
Body 必须提供一个静态数据成员，cond_value
Body 必须提供两个子类型定义 res_type 和 next_type

res_type 和 type 都用 typename 修饰，表明都是类型
*/



```