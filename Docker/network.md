# 网络

docker 在安装时会默认创建三个网络，可以使用 `docker network ls` 可以查看

docker 内部支持 4 种网络：

- bridge：桥接模式，它会为每一个容器分配、设置 IP，并把容器连接到 docker0 虚拟网桥，通过 docker0 网桥以及 iptables nat 表配置与宿主机通信；

- host：容器和宿主机共享 Network namespace，也就是容器没有内部模式；

- none：容器有独立的 Network namespace，但并没有对其进行任何网络设置，如分配 veth pair 和网桥连接、配置 IP 等；

- container：容器和另外的容器共享 Network namespace，在 k8s 中的 pod 就是多个容器共享一个 Network namespace

