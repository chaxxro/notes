# 函数

```sh
[function] funname [()] {
    action;
    [return int;]
}
# 可以带 function 定义，也可以直接定义
# 作为返回值，return 后跟数值 0-255
# 函数返回值在调用该函数后通过 $? 来获得

funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"

# 调用函数时可以向其传递参数
# 在函数体内部，通过 $n 的形式来获取参数的值
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

shell 会把函数当作一个小型脚本，运行结束时会返回一个退出状态码

- 默认情况下，函数的退出状态码是函数中最后一条命令返回的退出状态码，在函数执行结束后，可以用标准变量 `$?` 来确定函数的退出状态码

- 使用 `return` 命令来退出函数并返回特定的退出状态码，退出状态码必须是 0~255

```sh
aaa=1000

arg_proc () {
	echo "Function begin:"
	local aaa=2000
	echo $1
	echo $2
	echo $3
	echo $*
	echo $@
	echo $aaa
	echo "Function end!"
}

echo "Script bugin:"
echo $1
echo $2
echo $3
echo $*
echo $@
echo $aaa

arg_proc aaa bbb ccc ddd eee fff

echo $1
echo $2
echo $3
echo $*
echo $@
echo $aaa
echo "Script end!"
```

函数参数跟函数内部使用 `local` 定义的局部变量效果一样，都是只在函数内部能看到，函数外部看不到函数里定义的局部变量

- 当函数内部的局部变量和外部的全局变量名字相同时，函数内只能取到局部变量的值

- 当函数内部没有定义跟外部同名的局部变量的时候，函数内部也可以看到全局变量
