# decltype

`decltype` 用于生成变量名或者表达式的类型，却不实际计算表达式的值

大多数情况下，与使用模板和 `auto` 时进行的类型推断相比，`decltype` 作用于变量名或者表达式只是重复了一次变量名或者表达式的确切类型

- 表达式是标识符、类访问表达式，`decltype(exp)` 和表达式类型一致

- 表达式是函数调用， `decltype(exp)` 和返回值的类型一致；若函数返回的是纯右值，只有类类型可以携带 `const/volatile`，此外都忽略掉

- 若表达式是一个左值，则 `decltype(exp)` 是左值引用

```cpp
const int i = 0;  
// decltype(i) 为 const int
int arr[5];
// decltype(arr) 为 arr[]
int* ptr;
// decltype(ptr) 为 int*

bool f(const Widget& w);
// decltype(w) 为 const Widget&
// decltype(f) 为 bool(const Widget&)

struct Point {
    int x, y; 
};
// decltype(Point::x) 为 int
// decltype(Point::y) 为 int

Widget w;
// decltype(w) 为 Widget

if (f(w)) {}
// decltype(f(w)) 为 bool

template<typename T>
class vector { 
public:
T& operator[](std::size_t index);
};
vector<int> v;
// decltype(v) 为 vector<int> 
if (v[0] == 0) {}
// decltype(v[0]) 为 int&
```

在泛型编程中，`auto` 和 `decltype` 结合使用达到返回类型后置

```cpp
template < typename T, typename U>
auto add(T t, U u) -> decltype(t+u)
{
	return t + u;
}

int main(int argc, _TCHAR* argv[])
{
	int a = 1; float b = 2.0;
	auto c = add(a, b);
	return 0;
}
```