# thread_local

存储器指定符指定了变量名的存储期以及链接方式

- auto：自动存储期

- register：自动存储期，提示编译器将此变量置于寄存器中

- static：静态或线程存储期，同时提示是内部链接

- extern：静态或线程存储期，同时提示是外部链接

- thread_local：线程存储期

- mutable

`thread_local` 只允许用于声明在命名空间作用域的全局对象、声明于块作用域的本地对象及类的静态成员

```cpp
thread_local int x;  //A thread-local variable at namespace scope

class X {
    //A thread-local static class data member
    static thread_local std::string s; 
};
//The definition of X::s is required
static thread_local std::string X::s;  

void foo() {
    thread_local std::vector<int> v;  //A thread-local local variable
}
```

`thread_local` 关键字修饰的变量具有线程周期，这些变量在线程开始的时候被生成，在线程结束的时候被销毁，并且每一个线程都拥有一个独立的变量实例

`thread_local` 能与 `static` 或 `extern` 结合，以分别指定内部或外部链接（除了静态成员，静态成员始终拥有外部链接），但附加的 `static` 不影响存储期

`static thread_local` 和 `thread_local` 声明是等价的