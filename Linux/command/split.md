# split

分割文件

```
split [-a suffix_length] [-b byte_count[k|m]] [-l line_count] [-d] [file [name]]

line_count: 指定每多少行切成一个小文件
byte_count: 指定每多少字节切成一个小文件，单位 M 和 kb
-d: 使用数字作为后缀
-a: 指定后缀长度
```