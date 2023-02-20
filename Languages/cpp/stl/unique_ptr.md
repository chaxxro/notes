# unique_ptr

`unique_ptr` 是一个独享所有权的智能指针

```cpp
template<class T, class Deleter = std::default_delete<T>>
class unique_ptr;

template <class T, class Deleter>
class unique_ptr<T[], Deleter>;
```

把拷贝构造和赋值函数 `delete`，任何试图调用它的行为将产生编译期错误，不允许 `unique_ptr` 进行拷贝、赋值，但是可以进行移动构造和移动赋值

```cpp
// 可指定删除器
class Socket {
public:
	Socket() {}
	~Socket() {}
	void close() {}
}

auto deletor = [](Socket* pSocket) {
	pSocket->close();
	delete pSocket;
}
std::unique_ptr<Socket, void (*)(Socket*)> sp(new Socket(), deletor);
// 使用 decltye
std::unique_ptr<Socket, decltype(deletor)> sp(new Socket(), deletor);
```