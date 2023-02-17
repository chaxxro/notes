# grep

全面搜索正则表达式并把行打印出来

用基本的 Unix 风格正则表达式来匹配模式

```sh
grep [-abcdDEFGHhIiJLlMmnOopqRSsUVvwXxZz] [-A num] [-B num] [-C[num]]
     [-e pattern] [-f file] [--binary-files=value] [--color[=when]]
     [--colour[=when]] [--context[=num]] [--label] [--line-buffered]
     [--null] [pattern] [file ...]

# 不要忽略二进制数据
-a --text

# 除了显示符合范本样式的那一行之外，并显示该行之后的内容
-A num   --after-context=num

# 在显示符合范本样式的那一行之外，并显示该行之前的内容
-b --byte-offset

# 除了显示符合样式的那一行之外，并显示该行之前的内容
-B num   --before-context=num

# 计算符合范本样式的行数
-c --count

# 除了显示符合范本样式的那一行之外，并显示该行前后的内容
# 默认值是 2，等于 -A 2 -B 2
-Cnum --context=num

# 当指定要查找的是目录而非文件时，必须使用这项参数，否则 grep 命令将回报信息并停止动作
# 默认 read，skip 表示跳过目录，recurse 表示递归目录，等同于 -R -r
-d action --directories=action

# 当指定要查找的是设备、socket，必须使用这项参数，否则 grep 命令将回报信息并停止动作
# 默认 read，skip 表示跳过
-D action --devices=action

# 指定字符串作为查找文件内容的范本样式
-e pattern --regexp=pattern

# 将范本样式为延伸的普通表示法来使用，意味着使用能使用扩展正则表达式
-E --extended-regexp

# 排除带绝对路径文件名匹配 pattern 的文件
--exclude pattern

# 如果指定了 -R，排除目录
--exclude-dir pattern

-f file, --file=file
-F --fixed-regexp 

# 将范本样式视为普通的表示法来使用
-G --basic-regexp

# 在显示符合范本样式的那一行之前，不标示该列所属的文件名称
-h --no-filename

# 在显示符合范本样式的那一行之前，标示该列的文件名称
-H --with-filename

# 忽略字符大小写的差别
-i --ignore-case

# 忽略二进制文件，等同于 --binary-file=without-match
-I

# 只搜索带绝对路径文件名匹配 pattern 的文件
--include pattern

# 如果指定了 -R，只搜索匹配上的目录
--include-dir pattern

# 列出文件内容符合指定的范本样式的文件名称
-l --file-with-matches

# 列出文件内容不符合指定的范本样式的文件名称
-L --files-without-match

# 指定最大匹配数
-m num, --max-count=num

# 在显示符合范本样式的那一行之前，标示出该列的编号
-n --line-number

# 只输出文件中匹配到的部分
-o

# 跟随软链
-O

# 不跟随软链
-p

# 不显示任何信息
-q --quiet或--silent

# 此参数的效果和指定 -d recurse 参数相同
-R/-r  --recursive

# 不显示错误信息
-s --no-messages

# 反转查找，输出不包含匹配项的行
-v --revert-match

# 显示版本信息
-V --version

# 只显示全字符合的列
-w --word-regexp

# 只显示全列符合的列
-x --line-regexp
```

```sh
grep '^\.Pp' myfile
grep -v -e 'foo' -e 'bar' myfile
grep -H -R FIXME --include=*.h /usr/src/sys/arm/
```