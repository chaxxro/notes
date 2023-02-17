# 信息变量

## 地址变量

变量名|描述|
-|-|
CMAKE_PROJECT_NAME、CMAKE_PROJECT_DESCRIPTION、CMAKE_PROJECT_HOMEPAGE_URL、CMAKE_PROJECT_VERSION|顶层 `project` 设置的工程属性
PROJECT_NAME、PROJECT_DESCRIPTION、PROJECT_HOMEPAGE_URL、PROJECT_VERSION|最近 `project` 设置的工程属性
CMAKE_MODULE_PATH|`find_package` 首先搜索第三方库的路径
CMAKE_SOURCE_DIR|顶层 CMakeLists.txt 所在路径
CMAKE_CURRENT_SOURCE_DIR|当前处理的 CMakeLists.txt 所在的路径
CMAKE_BINARY_DIR|工程编译发生的目录
CMAKE_CURRRENT_BINARY_DIR|in-source 编译时等同于 CMAKE_CURRENT_SOURCE_DIR，否则是当前 CMakeLists.txt 将要生效的目录地址
CMAKE_CURRENT_LIST_FILE|当前 CMakeLists.txt 的带绝对地址的文件名
CMAKE_CURRENT_LIST_DIR|当前 CMakeLists.txt 所在文件的绝对路径
PROJECT-NAME_BINARY_DIR、PROJECT-NAME_SOURCE_DIR|工程对应的地址
PROJECT_BINARY_DIR|工程编译发生的目录
PROJECT_SOURCE_DIR|最近的使用 `project` 的目录地址
CMAKE_GENERATOR|当前使用的构建工具
CMAKE_MATCH_COUNT|匹配结果数量
CMAKE_MATCH_\<n\>|最近的一次正则匹配结果，n 从 0 到 9，0 表示所有匹配结果，1 到 9 表示匹配结果索引