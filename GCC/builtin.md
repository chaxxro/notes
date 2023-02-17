# builtin

## __builtin_ctz

返回输入数二进制表示从最低位开始(右起)的连续的 0 的个数

如果传入 0 则行为未定义

```cpp
int __builtin_ctz (unsigned int x)
int __builtin_ctzl (unsigned long)
int __builtin_ctzll (unsigned long long)
```

## __builtin_clz

返回输入数二进制表示从最高位开始(左起)的连续的 0 的个数

如果传入 0 则行为未定义

```cpp
int __builtin_clz (unsigned int x)
int __builtin_clzl (unsigned long)
int __builtin_clzll (unsigned long long)
```

## __builtin_ffs

返回输入数二进制表示的最低非 0 位的下标，下标从 1 开始计数

传入 0 则返回 0

```cpp
int __builtin_ffs (unsigned int x)
int __builtin_ffsl (unsigned long)
int __builtin_ffsll (unsigned long long)
```

## __bulitin_popcount

返回输入的二进制表示中 1 的个数

如果传入 0 则返回 0

```cpp
int __builtin_popcount (unsigned int x)
int __builtin_popcountl (unsigned long)
int __builtin_popcountll (unsigned long long)
```

## __bulitin_parity

返回输入的二进制表示中 1 的个数的奇偶，也就是输入的二进制中 1 的个数对 2 取模的结果

```cpp
int __builtin_parity (unsigned int x)
int __builtin_parityl (unsigned long)
int __builtin_parityll (unsigned long long)
```

## __buitin_expect

GCC 在编译过程中，会将可能性更大的代码紧跟着前面的代码，从而减少指令跳转带来的性能上的下降, 达到优化程序的目的

```cpp
long __builtin_expect(long exp, long c);
// exp 为一个整型表达式
// c 必须是一个编译期常量, 不能使用变量，表示期望 exp 表达式的值等于常量 c
// 返回值就是 exp 的值

// 使用 __builtin_expect 构建两个宏
#define likely(x)      __builtin_expect(!!(x), 1)
#define unlikely(x)    __builtin_expect(!!(x), 0)
```