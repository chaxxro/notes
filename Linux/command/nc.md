# nc

用于模拟一个服务器程序被其他客户端接入，或者模拟一个客户端连接其他服务器，连接之后可以发送数据

`nc` 默认使用 TCP，`-u` 使用 UDP

```sh
# 显示详细信息
nc -v

# 开启一个监听服务，方便客户端连接
nc -v -l 127.0.0.1 6000

# 模拟客户端
# -p 指定客户端的端口
nc -v 0.0.0.0 6000
nc -v -p 5555 0.0.0.0 6000

# 发送文件
nc 0.0.0.0 6000 < file

# 接收文件
nc -l 0.0.0.0 6000 > file
```

`nc` 默认使用 `\n` 作为消息的结束标志，`-C` 则使用 `\r\n` 作为消息结束标志