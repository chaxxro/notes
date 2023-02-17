# 枚举

## enum

普通 `enum` 作用域是全局的，会出现重定义的问题

无法指定底层所使用的数据类型

```cpp
enum Traffic_light{red, green,yellow};

//error: redefinition of enumerator 'red'
enum Warning{red, green, yellow, orange}; 
```

普通的 `enum` 和其基础类型存在隐式类型转换

## enum class

`enum class` 是限制了作用域的强类型枚举

枚举常用一些整数类型表示，每个枚举值是一个整数

可以把用于表示某个枚举的类型称为它的基础类型，基础类型默认是 `int`，但可以显示的指定

```cpp
enum class Traffic_light{red, yellow, green};

enum class Traffic_light : uint8_t {red, yellow, green};
```

`enum class` 不支持隐式类型转换