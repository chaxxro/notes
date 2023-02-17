## 锁

- 互斥锁 mutex：用于保证在任何时刻，都只能有一个线程访问该对象。当获取锁操作失败时，线程会释放 CPU；等到锁被释放后，内核会在合适的时机唤醒线程，当这个线程成功获取到锁后，于是就可以继续执行；存在两次线程上下文切换开销

- 读写锁 rwlock：分为读锁和写锁；处于读操作时，可以允许多个线程同时获得读操作；同一时刻只能有一个线程可以获得写锁，其它获取写锁失败的线程都会进入睡眠状态，直到写锁释放时被唤醒；写锁会阻塞其它读写锁，当有一个线程获得写锁在写时，读锁也不能被其它线程获取，写锁优先于读锁（一旦有写锁，则后续读者必须等待，唤醒时优先考虑写锁）；适用于读取数据的频率远远大于写数据的频率的场合

- 自旋锁 spinlock：在任何时刻同样只能有一个线程访问对象，但是当获取锁操作失败时，不会释放 CPU，而是会在原地自旋，直到锁被释放；保护的临界区必须小，且操作过程必须短；节省了线程从睡眠状态到被唤醒期间的消耗，在加锁时间短暂的环境下会极大的提高效率；加锁时间过长，则会非常浪费 CPU 资源；自旋锁在用户态完成加锁和解锁，不会主动产生线程上下文切换；被广泛应用于内核

- RCU(read-copy-update)：在修改数据时首先需要读取数据，然后生成一个副本，对副本进行修改，修改完成后再将老数据 update 成新的数据；读锁几乎不需要同步开销，既不需要获得锁，也不使用原子指令，不会导致锁竞争；对于写锁的同步开销较大，它需要复制被修改的数据，还必须使用锁机制同步并行其它写锁的修改操作；在有大量读操作，少量写操作的情况下效率非常高

## 测试和置位指令实现锁

特殊原子操作指令 —— 测试和置位（Test-and-Set）指令

```cpp
// 形式如下
int TestAndSet(int * old_ptr, int new) {
    int old = *old_ptr;
    *old_ptr = new;
    return old;
}
```

这些代码是原子执行，既可以测试旧值，又可以设置新值

### 忙等待

用 Test-and-Set 指令来实现忙等待锁

```cpp
stuct lock_t {
    // flag == 0 时锁未被持有
    // flag == 1 时锁被持有
    int flag;
}

void init(lock_t * lock) {
    lock->flag = 0;
}

void lock(lock_t * lock) {
    while(TestAndSet(lock, 1) == 1) ;
}

void unlock(lock_t * lock) {
    lock->flag = 0;
}
```

当获取不到锁时，线程就会一直 while 循环，不做任何事情，所以就被称为忙等待锁，也被称为自旋锁

自旋锁利用 CPU 周期，一直自旋直到锁可用；在单处理器上，需要抢占式的调度器，因为一个自旋的线程永远不会放弃 CPU

### 无忙等待

当没获取到锁的时候，就把当前线程放入到锁的等待队列，然后执行调度程序，把 CPU 让给其他线程执行

```cpp
stuct lock_t {
    int flag;
    queue_t * q;  // 等待队列
}

void init(lock_t * lock) {
    lock->flag = 0;
    queue_init(lock->q);
}

void lock(lock_t * lock) {
    while(TestAndSet(lock, 1) == 1) {
        // 保存现在运行线程 TCB，并插入等待队列
        // 设置该线程为等待状态
        // 调度
    }
}

void unlock(lock_t * lock) {
    if(lock->q != nullptr) {
        // 移除等待队列的队头元素
        // 将该线程的 TCB 插入到就绪队列
        // 设置线程为就绪状态
    }
    lock->flag = 0;
}
```

## 信号量实现

为每类共享资源设置一个信号量 s，其初值为 1，表示该临界资源未被占用

只要把进入临界区的操作置于 P(s) 和 V(s) 之间，即可实现进程/线程互斥

任何想进入临界区的线程，必先在互斥信号量上执行 P 操作，在完成对临界资源的访问后再执行 V 操作

由于互斥信号量的初始值为 1，故在第一个线程执行 P 操作后 s 值变为 0，表示临界资源为空闲，

可分配给该线程，使之进入临界区；第二个线程想进入临界区，也应先执行 P 操作，结果使 s 变为

负值，这就意味着临界资源已被占用，第二个线程被阻塞；第一个线程执行 V 操作，释放临界资源

而恢复 s 值为 0 后，才唤醒第二个线程，使之进入临界区，待它完成临界资源的访问后，
又执行 V 操作，使 s 恢复到初始值 1

通过 V 操作的加 1 唤醒一个线程使得达到线程同步效果

## 读写锁实现

```cpp
class RWLock {
public:
    RWLock() : m_readCount(0), m_writeCount(0), m_isWriting(false) {}
    virtual ~RWLock() = default;

    void lockWrite() {
        std::unique_lock<std::mutex> ul(m_lock);
        ++m_writeCount;
        m_writeCond.wait(ul, [=](){return !m_isWriting && m_readCount == 0});
        m_isWriting = true;
    }
    void unlockWrite() {
        std::unique_lock<std::mutex> ul(m_lock);
        m_isWriting = false;
        if(0 == (--m_writeCount)) {
            m_readCond.notify_all();
        }
        else {
            m_writeCond.notify_one();
        }
    }

    void lockRead() {
        std::unique_lock<std::mutex> ul(m_lock);
        m_readCond.wait(ul, [=](){return 0 == m_writeCount;});
        ++m_readCount;
    }
    void unlockRead() {
        std::unique_lock<std::mutex> ul(m_lock);
        if(0 == (--m_readCount) && m_writeCount > 0) {
            m_writeCond.notify_one();
        }
    }

private:
    volatile int m_readCount;
    volatile int m_writeCount;
    volatile bool m_isWriting;
    std::mutex m_lock;
    std::condition_variable m_readCond;
    std::condition_variable m_writeCond;
};

class ReadGuard {
 public:
    explicit ReadGuard(RWLock& lock) : m_lock(lock) {
        m_lock.lockRead();
    }
    virtual ~ReadGuard() {
        m_lock.unlockRead();
    }

 private:
    ReadGuard(const ReadGuard&);
    ReadGuard& operator=(const ReadGuard&);

 private:
    RWLock &m_lock;
};

class WriteGuard {
 public:
    explicit WriteGuard(RWLock& lock) : m_lock(lock) {
        m_lock.lockWrite();
    }
    virtual ~WriteGuard() {
        m_lock.unlockWrite();
    }

 private:
    WriteGuard(const WriteGuard&);
    WriteGuard& operator=(const WriteGuard&);

 private:
  RWLock& m_lock;
};
```

## 自旋锁

```cpp
class spin_lock {
private:
    atomic_flag flag;
public:
    spin_lock() = default;
    ~spin_lock() = default;

    void lock() {
        while(flag.test_and_set());
    }

    void unlock() {
        flag.clear();
    }
}
```