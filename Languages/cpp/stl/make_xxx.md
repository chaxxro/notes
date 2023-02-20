# make_xxx

## make_shared、make_unique

`std::make_shared` 模板函数，可以返回一个指定类型的 `std::shared_ptr`

`std::make_unique` 模板函数，可以返回一个指定类型的 `std::unique_ptr`

```cpp
shared_ptr<string> p1 = make_shared<string>(10, '9');  
 
shared_ptr<string> p2 = make_shared<string>("hello");  
 
shared_ptr<string> p3 = make_shared<string>(); 
```

- `std::make_xxxxx` 比起直接使用 `new`，效率更高

- 异常安全，发生异常时仍释放资源

- 构造函数是保护或私有时，无法使用 `std::make_xxxx`

- 对象的内存可能无法及时回收

- `std::make_xxxxx` 无法指定删除器