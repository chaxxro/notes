# enable_shared_from_this

`enable_shared_from_this` 是一个以其派生类为模板类型实参的基类模板，继承它可将 `this` 转化成 `shared_ptr`

```cpp
class B : public std::enable_shared_from_this<B> {
public:
	std::shared_ptr<B> GetSelf() {
		return shared_from_this();
	}
}

shared_ptr<B> ptr = make_shared<B>();
```

使用 `shared_from_this()` 获取 `this`

`shared_from_this()` 不能在构造函数中调用，因为对象还没转交给 `shared_ptr`

- 由于引用计数数据和原始数据不是存放在同一处，因此不能对非 `shard_ptr` 对象调用使用 `shared_from_this()` 的函数，`shared_from_this()`

```cpp
int main() {
	B b;
	auto sp = b.GetSelf();  // 奔溃
	return 0;
}
```

- 避免出现循环引用问题