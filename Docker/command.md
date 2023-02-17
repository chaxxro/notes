# 常用命令

## 安装 docker

```sh
# 安装
yum install docker-ce docker-ce-cli containerd.io

# 开机启动
systemctl enable docker
# 启动
systemctl start docker

# 测试
docker run --rm hello-world
```

## 使用国内源

```sh
# 查看是否在 docker.service 文件中配置过镜像地址
systemctl cat docker | grep '\-\-registry\-mirror'
# 有配置镜像地址

# 没有配置镜像地址
# 在 /etc/docker/daemon.json 中写入
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
  ]
}
```

## 卸载

```sh
# 停止 docker 服务
systemctl stop docker

# yum 移除 docker 相关组件
yum remove docker-ce docker-ce-cli containerd.io

# 删除 lib 目录下的 docker 内容
rm -rf /var/lib/docker

# 删除 lib 目录下的 contain 内容
rm -rf /var/lib/containerd
```

## 查看 docker 版本信息

```sh
docker version
```

## 查看 docker 概要信息

```sh
docker info
```

## 镜像命令


```sh
# 查看所有本地主机上的镜像
docker images [OPTIONS] [REPOSITORY[:TAG]]
# OPTION主要有两个：
# -a：显示所有的本地镜像（含历史映像层）
# -q：仅显示镜像ID

# 查询
docker search [OPTIONS] image-name
### 结果说明：
NAME：镜像名称
DESCRIPTION：镜像描述
STARS：打星的数量
OFFICIAL：是否官方，带有[OK]表示是官方镜像
AUTOMATED：是否自动编译
# OPTION 主要使用一个
# --limit int : 限制显示多少个
# --stars={x}：最少有x颗星
# --filter <fitler>：满足某种表达式条件，若有多个表达式，则使用多个--filter分割；
# 包含名称，自动构建，并且是{x}星以上，并且是官方版本：
--filter "stars=3" --filter "is-automated=true"  --filter "is-official=true" hello-world
```

## 拉取镜像

```sh
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
# tag 默认是 lasest
# name 是两段式名称，即 <用户名>/<软件名>
# 对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像
# OPTION 说明：
# -a : 下载所有的tag版本的镜像
# --disable-content-trust ：跳过镜像校验
```

## 查看镜像/容器/数据卷等所占空间

```sh
docker system df
### 执行结果
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          1         1         13.26kB   0B (0%)
Containers      2         0         0B        0B
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B
```

## 删除镜像

```sh
docker rmi [OPTIONS] IMAGE [IMAGE...]
# OPTION参数说明
# -f ：强制删除某个镜像

# 删除所有镜像
docker rmi -f $(docker images -qa)
```

## 更新镜像

```sh
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

# OPTIONS说明
# -a :提交的镜像作者
# -c :使用 Dockerfile 指令来创建镜像
# -m :提交时的说明文字
# -p :在 commit 时将容器暂停
```

## 设置标签

```sh
docker tag <镜像ID> <仓库名>:<版本号>
```

## 导出镜像

```sh
docker save <镜像ID> > <镜像文件名>.tar
```

## 导入镜像

```sh
docker load < <镜像文件名>.tar
```

## 启动容器

```sh
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

# option
-a 登录后台运行的容器
-d 指定容器运行于前台还是后台
-c 设置容器CPU权重，在CPU共享场景使用
-e, --env=[] 指定环境变量，容器中可以使用该环境变量
-h, --hostname="" 指定容器的主机名
-i, --interactive=false 打开STDIN，用于控制台交互
-t, --tty=false 分配tty设备，该可以支持终端登录
-m, --memory="" 指定容器的内存上限
-P, --publish-all=false 指定容器暴露的端口
-p, --publish=[] 指定容器暴露的端口
-u, --user="" 指定容器的用户
-v, --volume=[] 给容器挂载存储卷，挂载到容器的某个目录
-w, --workdir="" 指定容器的工作目录
--name="" 指定容器名字，后续可以通过名字进行容器管理，links特性需要使用名字
--cap-add=[] 添加权限，权限清单详见：http://linux.die.net/man/7/capabilities
--cap-drop=[] 删除权限，权限清单详见：http://linux.die.net/man/7/capabilities
--cidfile="" 运行容器后，在指定文件中写入容器PID值，一种典型的监控系统用法
--cpuset="" 设置容器可以使用哪些CPU，此参数可以用来容器独占CPU
--device=[] 添加主机设备给容器，相当于设备直通
--dns=[] 指定容器的dns服务器
--dns-search=[] 指定容器的dns搜索域名，写入到容器的/etc/resolv.conf文件
--entrypoint="" 覆盖image的入口点
--env-file=[] 指定环境变量文件，文件格式为每行一个环境变量
--expose=[] 指定容器暴露的端口，即修改镜像的暴露端口
--link=[] 指定容器间的关联，使用其他容器的IP、env等信息
--lxc-conf=[] 指定容器的配置文件，只有在指定--exec-driver=lxc时使用
--net="bridge" 容器网络设置
--privileged=false 指定容器是否为特权容器，特权容器拥有所有的capabilities
--restart="" 指定容器停止后的重启策略
--rm=false 指定容器停止后自动删除容器(不支持以docker run -d启动的容器)
--sig-proxy=true 设置由代理接受并处理信号，但是SIGCHLD、SIGSTOP和SIGKILL不能被代理
```

## 退出容器

```sh
# 直接退出，该退出会导致容器退出
exit
# 后台退出，该退出仅仅是退出交互终端，不会导致容器退出
# 重新进入可使用：docker attach
Ctrl + p + q
```

## 重新进入容器

`docker attach` 进入容器正在执行的终端，不会启动新的进程

```sh
# 连接到正在运行中的容器
docker attach [OPTIONS] CONTAINER
# --sig-proxy=false 来确保 CTRL-D 或 CTRL-C 不会关闭容器
```

`docker exec` 进入容器后开启一个新的终端，可以在里面操作

```sh
docker exec -it container /bin/bash
```

## 重启容器

```sh
# 重启容器
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

## 删除容器

```sh
docker rm container
# 不能删除正在运行的容器，如果强制删除 rm -f

# 删除全部已停止的容器
docker rm $(docker ps -aq)
# 删除全部容器，不管是否已停止
docker rm -f $(docker ps -aq)
# 删除已经退出的容器
docker rm $(docker ps -a | grep Exited | awk -F " " '{print $1}')
```

## 查看容器

```sh
docker ps [OPTIONS]
# 常用参数说明：
# -a：所有容器（不加的话表示默认，仅显示活着的容器）
# -n：显示最近的几个，使用方式：docker ps -n <x>
# -l：显示最新的容器
# -q：只显示容器ID
```

## 启动容器

```sh
#  启动一个或多个已经被停止的容器
docker start [OPTIONS] CONTAINER [CONTAINER...]
```

## 暂停容器

```sh
docker stop [OPTIONS] CONTAINER [CONTAINER...]
# 常用参数说明
# -t : 停止之前等待多少秒，-t 10 表示10s后再停止
```

## 查看容器占用情况

```sh
docker stats [OPTIONS] [CONTAINER...]
```

## 查看容器底层信息

```sh
docker inspect CONTAINER
```