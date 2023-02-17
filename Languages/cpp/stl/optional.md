# optional

`std::nullopt` 是没有值的 `optional` 的表达形式，等同于 `{ }`

```cpp
// 空 optiolal
optional<int> oEmpty;
optional<float> oFloat = nullopt;

optional<int> oInt(10);
optional oIntDeduced(10);  // type deduction

// make_optional
auto oDouble = std::make_optional(3.0);
auto oComplex = make_optional<complex<double>>(3.0, 4.0);

// in_place
optional<complex<double>> o7{in_place, 3.0, 4.0};

// initializer list
optional<vector<int>> oVec(in_place, {1, 2, 3});

// 拷贝赋值
auto oIntCopy = oInt;
```

`has_value()` 和 `operator()` 返回是否有值，`value()` 返回这个值

如果没有值并且还调用 `value()`，会抛出一个类型为 `std::bad_optional_access` 的异常

`value_or` 来设置没有值时的默认返回值