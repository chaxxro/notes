# file

文件操作命令

## 读

```cmake
file(READ <filename> <variable>
     [OFFSET <offset>] [LIMIT <max-in>] [HEX])
```

- `offset` 开始位置的偏移量

- `max-in` 读取的最多字节数

- `HEX` 是否转换成十六进制

```cmake
file(STRINGS <filename> <variable> [<options>...])
```

从文件中读取 ASCII 字符串列表

二进制数据和 `\r` 都会被忽略

可选项：

- `LENGTH_MAXIMUM <max-len>` 字符串的长度最大值

- `LENGTH_MINIMUM <min-len>` 字符串的长度最小值

- `LIMIT_COUNT <max-num>` 获取的不同字符串的数量

- `LIMIT_INPUT <max-in>` 从文件中读取的最多字节数

- `LIMIT_OUTPUT <max-out>` 列表中最多储存的字节数

- `NEWLINE_CONSUME` 将 `\r` 作为字符的一部分

- `REGEX <regex>` 字符串满足正则表达式

- `ENCODING <encoding-type>` 支持 UTF-8, UTF-16LE, UTF-16BE, UTF-32LE, UTF-32BE

```cmake
file(<HASH> <filename> <variable>)
```

读取文件内容的哈希值

## 写

```cmake
file(WRITE <filename> <content>...)
file(APPEND <filename> <content>...)
```

`WRITE` 会覆盖原有文件内容，`APPEND` 在原有内容后追加新的内容

```cmake
file(TOUCH [<files>...])
file(TOUCH_NOCREATE [<files>...])
```

`TOUCH` 新建文件，若文件存在则更新其属性

`TOUch_NOCREATE` 不新建文件

## 文件操作

```cmake
file(GLOB <variable>
     [LIST_DIRECTORIES true|false] [RELATIVE <path>] [CONFIGURE_DEPENDS]
     [<globbing-expressions>...])
file(GLOB_RECURSE <variable> [FOLLOW_SYMLINKS]
     [LIST_DIRECTORIES true|false] [RELATIVE <path>] [CONFIGURE_DEPENDS]
     [<globbing-expressions>...])
```

生成满足 `globbing-expressions` 的文件名列表，并将其存储到变量中

- `RELATIVE` 文件名取与 `path` 的相对地址

- `LIST_DIRECTORIES` 是否忽略目录

- `CONFIGURE_DEPENDS` 每次都更新该变量值，确保不遗漏新增文件，但不建议使用

```cmake
file(MAKE_DIRECTORY [<directories>...])
```

新建文件夹

```cmake
file(REMOVE [<files>...])
file(REMOVE_RECURSE [<files>...])
```

移除文件

`REMOVE_RECURSE` 移除文件和文件夹

```cmake
file(RENAME <oldname> <newname>
     [RESULT <result>]
     [NO_REPLACE])
```

重命名文件和文件夹

- `RESULT` 成功时将 `result` 置为 0，否则重命名失败；不使用 `RESULT` 时，重命名失败会触发错误

- `NO_REPLACE` 若 `newname` 已存在则不替换

```cmake
file(COPY_FILE <oldname> <newname>
     [RESULT <result>]
     [ONLY_IF_DIFFERENT])
```

复制文件

- `RESULT` 成功时将 `result` 置为 0，否则重命名失败；不使用 `RESULT` 时，重命名失败会触发错误

- `ONLY_IF_DIFFERENT` 当 `newname` 已存在时，之前 `oldname` 与 `newname` 不同时才覆盖

```cmake
file(SIZE <filename> <variable>)
```

将文件大小存储在变量中

```cmake
file(CHMOD <files>... <directories>...
    [PERMISSIONS <permissions>...]
    [FILE_PERMISSIONS <permissions>...]
    [DIRECTORY_PERMISSIONS <permissions>...])
```

修改文件的权限，支持 `OWNER_READ`、`OWNER_WRITE`、`OWNER_EXECUTE`、`GROUP_READ`、`GROUP_WRITE`、`GROUP_EXECUTE`、`WORLD_READ`、`WORLD_WRITE`、`WORLD_EXECUTE`、`SETUID`、`SETGID`

## 路径转换

```cmake
file(REAL_PATH <path> <out-var> [BASE_DIRECTORY <dir>] [EXPAND_TILDE])
```

计算给定地址的绝对地址

- `BASE_DIRECTORY <dir>` 当 `path` 是相对地址时指定其基地址，默认使用 `CMAKE_CURRENT_SOURCE_DIR`

- `EXPAND_TILDE`

```cmake
file(RELATIVE_PATH <variable> <directory> <file>)
```

计算从 `directory` 到 `file` 的相对地址

## 转换操作

```cmake
file(DOWNLOAD <url> [<file>] [<options>...])
file(UPLOAD   <file> <url> [<options>...])
```

下载、上传文件

- `INACTIVITY_TIMEOUT <seconds>` 指定断开连接时间

- `LOG <variable>` 存储日志

- `SHOW_PROGRESS` 打印进度条

- `STATUS <variable>` 存储操作结果

- `TIMEOUT <seconds>` 指定操作的总时间

- `USERPWD <username>:<password>` 指定用户名和密码