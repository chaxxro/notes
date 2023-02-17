# 原子操作

当客户端需要修改数据时，基本流程分成两步：客户端先把数据读取到本地，在本地进行修改；客户端修改完数据后，再写回 Redis

读取-修改-写回操作（Read-Modify-Write，简称为 RMW 操作）

当有多个客户端对同一份数据执行 RMW 操作的话，就需要让 RMW 操作涉及的代码以原子性方式执行；访问同一份数据的 RMW 操作代码，就叫做临界区代码

```
current = GET(id)
current--
SET(id, current)
// 当有多个客户端执行这段代码时，这就是一份临界区代码
```

对临界区代码的执行没有控制机制，就会出现数据更新错误

![](../../Picture/Redis/note/atomic/01.png)

并发访问控制，是指对多个客户端访问操作同一份数据的过程进行控制，以保证任何一个客户端发送的操作在 Redis 实例上执行时具有互斥性

Redis 的快照生成、AOF 重写这些操作，可以使用后台线程或者是子进程执行，也就是和主线程的操作并行执行。这些操作只是读取数据，不会修改数据，所以，并不需要对它们做并发控制

虽然加锁保证了互斥性，但是加锁也会导致系统并发性能降低

为了实现并发控制要求的临界区代码互斥执行，Redis 的原子操作采用了两种方法

1. 单命令操作

2. 单个 Lua 脚本

## 单命令操作

把多个操作在 Redis 中实现成一个操作，也就是单命令操作

Redis 是使用单线程来串行处理客户端的请求操作命令的，所以，当 Redis 执行某个命令操作时，其他命令是无法执行的，这相当于命令操作是互斥执行的

虽然 Redis 的单个命令操作可以原子性地执行，但是在实际应用中，数据修改时可能包含多个操作，至少包括读数据、数据增减、写回数据三个操作，这显然就不是单个命令操作了；Redis 提供的 INCR/DECR 命令，把这三个操作转变为一个原子操作了。INCR/DECR 命令可以对数据进行增值 / 减值操作，而且它们本身就是单个命令操作，Redis 在执行它们时，本身就具有互斥性

所以如果执行的 RMW 操作是对数据进行增减值的话，Redis 提供的原子操作 INCR 和 DECR 可以直接帮助我们进行并发控制

## Lua 脚本

如果要执行的操作不是简单地增减数据，而是有更加复杂的判断逻辑或者是其他操作，那么，Redis 的单命令操作已经无法保证多个操作的互斥执行，所以需要使用第二个方法，也就是 Lua 脚本

Redis 会把整个 Lua 脚本作为一个整体执行，在执行的过程中不会被其他命令打断，从而保证了 Lua 脚本中操作的原子性

如果把很多操作都放在 Lua 脚本中原子执行，会导致 Redis 执行脚本的时间增加，同样也会降低 Redis 的并发性能

在编写 Lua 脚本时，你要避免把不需要做并发控制的操作写入脚本中