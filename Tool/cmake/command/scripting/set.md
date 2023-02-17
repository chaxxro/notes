# set

## 变量作用域

在 `function` 中定义的变量，变量仅在函数内和函数调用中可见

非 `function` 内定义变量的默认变量作用域是文件夹作用域，在处理当前文件夹下的 CMakeLists.txt 时，会将夫目录中的变量拷贝至当前目录中

全局变量，仅能被显式修改

## 普通变量

```cmake
set(<variable> <value>... [PARENT_SCOPE])
```

作用域是当前函数作用域或当前文件夹作用域

如果使用 `PARENT_SCOPE` 则会修改父目录中 CMakeLists.txt 的该变量值，而不会修改当前作用域该变量的值

## 缓存变量

```cmake
set(<variable> <value>... CACHE <type> <docstring> [FORCE])

set(CACHE_VAL "666" CACHE STRING INTERNAL)
```

相当于一个全局变量

默认情况下，不能覆盖已存在的缓存变量，除非使用 `FORCE`

`type` 包括：

- `BOOL`：`ON` 和 `OFF`

- `FILEPATH`：文件地址

- `PATH`：文件夹地址

- `STRING`：字符串

- `INTERNAL`：内部变量，不显示在 cmake-gui 

`doctring`：提示文字

## 环境变量

```cmake
set(ENV{<variable>} [<value>])

set(ENV{DEFINE} DEFINE)
```

使用格式 `$ENV{DEFINE}`

## 列表

列表也是字符串，这个变量有多个值，每个值用分号 `;` 进行分隔

不加引号的引用，cmake 将自动在分号处进行切分成多个列表元素，并把它们作为多个独立的参数传给命令

加了引号的引用 cmake 不会进行切分并保持分号不动，整个引号内的内容当作一个参数传给命令

```cmake
set(list_var 1 2 3 4) # list_var = 1;2;3;4
set(list_foo "5;6;7;8") # list_foo = 5;6;7;8
message(${list_var})#输出： 1234
message(${list_foo})#输出：5678
message("${list_var}")#输出：1;2;3;4
message("${list_foo}")#输出：5;6;7;8
```

# unset

复原变量

```cmake
unset(<variable> [CACHE | PARENT_SCOPE])

unset(ENV{<variable>})
```

- `CACHE`：删除一个 cache 变量而不是普通变量

- `PARENT_SCOPE`：删除夫目录中的当前变量