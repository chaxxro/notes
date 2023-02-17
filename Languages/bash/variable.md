# 变量

## 引号

双引号 `""` 可识别转义和变量

单引号 `''` 不识别转义合变量，原样输出

反引号 `` `` `` 用于执行命令

## 常用变量

`$?` 上一个命令的退出状态

`$0` 当前脚本的名称

`$$` 当前脚本 pid

`$1~n` 脚本或函数的传参

`$#` 脚本或函数的参数个数

`$@` 脚本或函数的所有传参

`$*` 脚本或函数的所有传参

`$@` 和 `$*` 都表示传入的参数，当 `$@` 或 `$*` 不被双引号包含时，都以 `$1 $2 ... $n` 形式输出，但当被双引号包含时，`"$*"` 将所有参数作为一个整体输出，而 `"$@"` 还是会将各个参数分割开

## 变量

```sh
# 三种定义变量方式
variable=value
variable='value'
variable="value"
# 如果 value 不包含任何空白符（例如空格、Tab 缩进等），那么可以不使用引号
# 否者需要使用引号包围
# 赋值号 = 的周围不能有空格
var=xx${variable} # xxvalue
```

使用一个定义过的变量，只要在变量名前面加美元符号 `$` 和花括号 `{}` 即可

```sh
author="xxx"
echo ${author}
skill="Java"
echo "I am good at ${skill}Script"
```

已定义的变量，可以被重新赋值

```sh
url="http://www.bupt.edu.cn"
website1='${url}'
website2="${url}"
echo $website1 # ${url}
echo $website2 # www.bupt.edu.cn
```

Shell 支持将命令的执行结果赋值给变量

```sh
variable=`command`
variable=$(command) # 支持嵌套，推荐使用该形式
```

使用 `unset` 命令可以删除变量

## 变量默认值

```sh
# 当 var 没有定义时，使用 defaultValue
# 而 var 依然为空，没有改变值
a=${var:-defaultValue}

# 当 var 没有定义时，使用 defaultValue
# var 也被赋值为 defaultValue
b=${var:=defaultValue}

# 当 var 没有定义时，或者定义了值为空，将在终端提示 value 并且退出
c=${var:?value}

# 当 var 定义并且不为空，将用 value 替换 var 的值
# 与 ${var:-value} 相反
d=${vari:+value}
```

## 字符串变量操作

`${#var}` 获取字符串变量长度

`${var:n}` 字符串提取；若 n 为正数，n 从 0 开始，表示提取第 n 个字符到末尾的字符；若 n 为负数，表示提取后 n 个字符

`${var:n1:n2}` 表示提取下标 n1 开始长度 n2 的字符串