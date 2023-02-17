
# 互斥量

## mutex

用于提供对共享变量的互斥访问 `#include <mutex>`

名称|用途
:-:|:-:
std::mutex|最基本也是最常用的互斥类
std::recursive_mutex|允许同一个线程对互斥量多次上锁（即递归上锁），来获得对互斥量对象的多层所有权
std::timed_mutex|除具备 mutex 功能外，还提供了带时限请求锁定的功能
std::recursive_timed_mutex|同一线程可递归的 timed_mutex

与 `std::thread` 一样，`mutex` 相关类不支持拷贝构造、不支持赋值，同时 `mutex` 类也不支持 `move` 语义(move 构造、move 赋值)

## mutex 通用操作

`lock`、`try_lock`、`unlock` 四个 `mutex` 都支持这些操作，但是不同类在行为上有些微区别

- `lock` 锁住 `mutex`

- 如果互斥量没有被锁住，则调用线程将该 `mutex` 锁住，直到调用 `unlock` 之前，该线程一直拥有该锁

- 如果 `mutex` 已被其它线程 `lock`，则调用线程将被阻塞，直到其它线程 `unlock` 该 `mutex`

- 如果当前 `mutex` 已经被当前线程锁住，则 `std::mutex` 产生死锁，而 `recursive` 系列则成功返回

## `try_lock` 尝试锁住 `mutex`

- 如果互斥量没有被锁住，则调用线程将该 `mutex` 锁住（返回 `true`），直到该线程调用 `unlock` 释放

- 如果 `mutex` 已被其它线程 `lock`，则调用线程将失败，并返回 `false`，但不会被阻塞

- 如果当前 `mutex` 已经被当前线程锁住，则 `std::mutex` 死锁，而 `recursive` 系列则成功返回 `true`

## `unlock` 解锁 `mutex`

释放对 `mutex` 的所有权

对于 `recursive` 系列 `mutex`，`unlock` 次数需要与 `lock` 次数相同才可以完全解锁

```cpp
#include <iostream>
#include <thread>
#include <mutex>

void inc(std::mutex &mutex, int loop, int &counter) {
    for (int i = 0; i < loop; i++) {
        mutex.lock();
        ++counter;
        mutex.unlock();
    }
}
int main() {
    std::thread threads[5];
    std::mutex mutex;
    int counter = 0;

    for (std::thread &thr: threads) {
        thr = std::thread(inc, std::ref(mutex), 1000, std::ref(counter));
    }
    for (std::thread &thr: threads) {
        thr.join();
    }

    // 输出：5000，如果inc中调用的是try_lock，则此处可能会<5000
    std::cout << counter << std::endl;
    return 0;
}
```

## 仅用于 timed 系列操作

`try_lock_for`、`try_lock_until` 仅用于 `timed` 系列的 mutex（`std::timed_mutex`,`std::recursive_timed_mutex`），函数最多会等待指定的时间，如果仍未获得锁，则返回 `false`。除超时设定外，这两个函数与 `try_lock` 行为一致

```cpp
// 等待指定时长
template <class Rep, class Period>
    try_lock_for(const chrono::duration<Rep, Period>& rel_time);
// 等待到指定时间
template <class Clock, class Duration>
    try_lock_until(const chrono::time_point<Clock, Duration>& abs_time);
```

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <chrono>

void run500ms(std::timed_mutex &mutex) {
    auto _500ms = std::chrono::milliseconds(500);
    if (mutex.try_lock_for(_500ms)) {
        std::cout << "获得了锁" << std::endl;
    } else {
        std::cout << "未获得锁" << std::endl;
    }
}
int main() {
    std::timed_mutex mutex;

    mutex.lock();
    std::thread thread(run500ms, std::ref(mutex));
    thread.join();
    mutex.unlock();

    return 0;
}
```

## 易用性类
在常规 `mutex` 的基础上，还提供了一些易用性的类

### lock_guard

`lock_guard` 利用了 C++ RAII 的特性，在构造函数中上锁，析构函数中解锁，从而保证了一个已锁的互斥量总是会被正确的解锁

```cpp
template <class Mutex> class lock_guard

/*
Mutex 代表互斥量，可以是 std::mutex、std::timed_mutex、std::recursive_mutex、std::recursive_timed_mutex 中的任何一个，也可以是 std::unique_lock，都提供了 lock 和 unlock 的能力

lock_guard 仅用于上锁、解锁，不对 mutex 承担供任何生周期的管理，因此在使用的时候，请确保 lock_guard 管理的 mutex 一直有效
*/

lock_guard(lock_guard const&) = delete;
lock_guard& operator=(lock_guard const&) = delete;
```

```cpp
std::mutex my_lock;

void add(int &num, int &sum)
{
    while(true)
    {
        std::lock_guard<std::mutex> lock(my_lock);  
        if (num < 100)
        { 
            //运行条件
            num += 1;
            sum += num;
        }   
        else 
        {  
            //退出条件
            break;
        }   
    }   
}

int main()
{
    int sum = 0;
    int num = 0;
    std::vector<std::thread> ver;   //保存线程的vector
    for(int i = 0; i < 20; ++i)
    {
        std::thread t = std::thread(add, std::ref(num), std::ref(sum));
        ver.emplace_back(std::move(t)); //保存线程
    }   

    std::for_each(ver.begin(), ver.end(), std::mem_fn(&std::thread::join)); //join
    std::cout << sum << std::endl;
}
```

### unique_lock

`lock_guard` 提供了简单上锁、解锁操作，但当我们需要更灵活的操作时便无能为力了

`unique_lock` 拥有对 `Mutex` 的所有权，一但初始化了 `unique_lock`，其就接管了该 `mutex`，在 `unique_lock` 结束生命周期前（析构前），其它地方就不要再直接使用该 `mutex` 了

```cpp
template <class Mutex>
class unique_lock
{
public:
    typedef Mutex mutex_type;
    // 空unique_lock对象
    unique_lock() noexcept;
    // 管理m, 并调用m.lock进行上锁，如果m已被其它线程锁定，由该构造了函数会阻塞。
    explicit unique_lock(mutex_type& m);
    // 仅管理m，构造函数中不对m上锁。可以在初始化后调用lock, try_lock, try_lock_xxx系列进行上锁。
    unique_lock(mutex_type& m, defer_lock_t) noexcept;
    // 管理m, 并调用m.try_lock，上锁不成功不会阻塞当前线程
    unique_lock(mutex_type& m, try_to_lock_t);
    // 管理m, 该函数假设m已经被当前线程锁定，不再尝试上锁。
    unique_lock(mutex_type& m, adopt_lock_t);
    // 管理m, 并调用m.try_lock_unitil函数进行加锁
    template <class Clock, class Duration>
        unique_lock(mutex_type& m, const chrono::time_point<Clock, Duration>& abs_time);
    // 管理m，并调用m.try_lock_for函数进行加锁
    template <class Rep, class Period>
        unique_lock(mutex_type& m, const chrono::duration<Rep, Period>& rel_time);
    // 析构，如果此前成功加锁(或通过adopt_lock_t进行构造)，并且对mutex拥有所有权，则解锁mutex
    ~unique_lock();

    // 禁止拷贝操作
    unique_lock(unique_lock const&) = delete;
    unique_lock& operator=(unique_lock const&) = delete;

    // 支持move语义
    unique_lock(unique_lock&& u) noexcept;
    unique_lock& operator=(unique_lock&& u) noexcept;

    void lock();
    bool try_lock();

    template <class Rep, class Period>
        bool try_lock_for(const chrono::duration<Rep, Period>& rel_time);
    template <class Clock, class Duration>
        bool try_lock_until(const chrono::time_point<Clock, Duration>& abs_time);

    // 显示式解锁，该函数调用后，除非再次调用lock系列函数进行上锁，否则析构中不再进行解锁
    void unlock();

    // 与另一个unique_lock交换所有权
    void swap(unique_lock& u) noexcept;
    // 返回当前管理的mutex对象的指针，并释放所有权
    mutex_type* release() noexcept;

    // 当前实例是否获得了锁
    bool owns_lock() const noexcept;
    // 同owns_lock
    explicit operator bool () const noexcept;
    // 返回mutex指针，便于开发人员进行更灵活的操作
    // 注意：此时mutex的所有权仍归unique_lock所有，因此不要对mutex进行加锁、解锁操作
    mutex_type* mutex() const noexcept;
};
```

```cpp
std::mutex mtx;           // mutex for critical section
std::once_flag flag;

void print_block (int n, char c)
 {
    //unique_lock有多组构造函数, 这里std::defer_lock不设置锁状态
    std::unique_lock<std::mutex> my_lock (mtx, std::defer_lock);
    //尝试加锁, 如果加锁成功则执行
    //(适合定时执行一个job的场景, 一个线程执行就可以, 可以用更新时间戳辅助)
    if(my_lock.try_lock())
    {
        for (int i=0; i<n; ++i)
            std::cout << c;
        std::cout << '\n';
    }
}

void run_one(int &n)
{
    std::call_once(flag, [&n]{n=n+1;}); //只执行一次, 适合延迟加载; 多线程static变量情况
}

int main () 
{
    std::vector<std::thread> ver;
    int num = 0;
    for (auto i = 0; i < 10; ++i)
    {
        ver.emplace_back(print_block,50,'*');
        ver.emplace_back(run_one, std::ref(num));
    }

    for (auto &t : ver)
    {
        t.join();
    }
    std::cout << num << std::endl;
    return 0;
}
```

### std::call_once

保证 `call_once` 调用的函数只被执行一次，需要与 `std::once_flag` 配合使用

`std::once_flag` 被设计为对外封闭的，即外部没有任何渠道可以改变 `once_flag` 的值，仅可以通过`std::call_once` 函数修改

```cpp
#include <iostream>
#include <thread>
#include <mutex>

void initialize() {
    std::cout << __FUNCTION__ << std::endl;
}

std::once_flag of;
void my_thread() {
    std::call_once(of, initialize);
}

int main() {
    std::thread threads[10];
    for (std::thread &thr: threads) {
        thr = std::thread(my_thread);
    }
    for (std::thread &thr: threads) {
        thr.join();
    }
    return 0;
}
// 仅输出一次：initialize
```

### std::try_lock

当有多个 mutex 需要执行 `try_lock` 时，该函数提供了简便的操作，`try_lock` 会按参数从左到右的顺序，对 mutex 顺次执行 `try_lock` 操作,当其中某个 `mutex.try_lock()` 失败(返回 `false` 或抛出异常)时，已成功锁定的 mutex 都将被解锁
该函数成功时返回-1， 否则返回失败 mutex 的索引，索引从 0 开始计数

```cpp
template <class L1, class L2, class... L3>
  int try_lock(L1&, L2&, L3&...);
```

### std::lock
批量上锁方式，采用死锁算法来锁定给定的mutex列表，避免死锁，上锁顺序是不确定的
该函数保证: 如果成功，则所有mutex全部上锁，如果失败，则全部解锁

```cpp
template <class L1, class L2, class... L3>
  void lock(L1&, L2&, L3&...);
```