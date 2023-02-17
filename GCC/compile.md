# 编译过程

![](../Picture/GCC/g%2B%2B/01.png)

- 预处理：预处理器在正式编译前处理 C/C++ 文件中的预处理命令，C 语言预处理器生成 .i 文件，C++ 预处理器生成 .ii 文件

- 编译：编译器将预处理完的文件进行一系列词法分析、语法分析、语义分析及优化后成为相应的 .s 汇编代码文件

### 汇编

汇编器将汇编代码转变成机器可以执行的命令，生成相应的目标文件（Linux 为 .o 文件，Windows 为 .obj 文件）

### 链接

链接器将各个目标文件组装在一起，解决符号依赖、库依赖关系，并生成可执行文件

## 常用命令

`-o <file>`：输出编译后的结果到指定的文件 file 中

`-E`：对源文件进行预处理，预处理后生成 .i 或者是 .ii 文件，生成的是文本文件

`-S`：只进行预处理和编译

`-c`：只进行预处理，编译，汇编操作，生成 .o(.obj) 文件，不进行链接

`-g`：产生调试信息

`-gn`：生成调试信息,同时用 n 指出需要多少信息，默认值是 2

`-Dmacro`：添加宏 `macro`

`-Dmacro=defn`：定义宏 `macro` 的内容为 `defn`

`-Umacro`：取消宏

`-u`：移除所有预定义宏

`-std=`：选择语言标准，`-std=c++11`

`-IDIRECTORY`：指定额外的头文件搜索路径

`-LDIRECTORY`：指定额外的函数库搜索路径

`-lLIBRARY`：连接时搜索指定的函数库

`-v`：打印所有执行的命令

`-w`：编译时，不显示任何警告消息

`-Wall`：编译时，显示所有出现的警告消息

`-Werror`：将警告升级为错误

`-Wextra`：打印额外告警消息

`-Wconversion`：隐式转换可能会改变值时告警

`-Wold-style-cast`：使用 c 风格类型转换时报警

`-Wno-unused-parameter`：未使用变量报警

`-Woverloaded-virtual`：重写虚函数时应使用 `virtual`

`-Woverflow`：算术计算溢出时报警

`-Wpointer-arith`：算术表达式中使用指针时报警

`-Wshadow`：当一个局部变量覆盖另一个局部变量时报警

`-Wwrite_strings`：将字面字符串转换为 `char*` 报警

`-pedantic`：允许发出ANSI/ISO C标准所列出的所有警告

`-fPIC`：生成与位置无关的代码，用于动态链接库

`-fpic`：编译器就生成位置无关目标码，适用于静态库

`-funsigned-char`、`-fno-signed-char`、`-fsigned-char`、`-fno-unsigned-char`：这四个参数是对 `char` 类型进行设置,决定将 `char` 类型设置成 `unsigned char` 或者 `signed char`

`-fsanitize=address`：开启内存地址检查

`-fno-elide-constructors`：关闭 RVO 优化

## 优化等级

- -O0 关闭所有优化选项，默认等级

- -O1/O2/03 优化

- -Og 在 -O1 的基础上，取消了影响调试的优化，但这个参数只是告诉编译器不要影响调试，但调试信息的生成还是依赖 -g

- -Os 以 -O2 为基础，取消了可能导致程序变大的优化

- -Ofast 以 -O3 为基础，添加非常规优化