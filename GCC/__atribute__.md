# \_\_attribute\_\_

`__attribute__` 可以设置函数属性、变量属性和类型属性

```c
__attribute__ ((attribute-list))
```
## aligned

设定一个指定大小的对齐格式，以字节为单位

```c
// 指定以 8 字节对齐
struct S {
    short b[3];
} __attribute__ ((aligned (8)));

// 指定以最优方式对齐
struct S {
    short b[3];
} __attribute__ ((aligned));
```

`aligned` 属性使被设置的对象占用更多的空间

## packed

取消内存对齐，采用紧密分布方式

```c
struct x {
    int a;
    char b;
    struct p px;
    short c;
} __attribute__ ((packed))
```

`packed` 可以减小对象占用的空间

## format

指定一个函数比如 `printf` 或 `scanf`作为参数，使编译器检查函数声明和函数实际调用参数之间的格式化字符串是否匹配

```c
format (archetype, string-index, first-to-check)

extern int my_printf (void *my_object, const char *my_format, ...) __attribute__((format(printf, 2, 3)));
```

`string-index` 指定传入函数的第几个参数是格式化字符串

`first-to-check` 指定从函数的第几个参数开始按上述规则进行检查

当函数是成员函数时，由于编译器会添加 `this` 指针，所以 `first-to-check` 需要后移一位

## noreturn

通知编译器函数从不返回值，当遇到类似函数需要返回值而却不可能运行到返回值处就已经退出来的情况，该属性可以避免出现错误信息

```c
extern void exit(int) __attribute__((noreturn));
```