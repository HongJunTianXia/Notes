```
$ docker ps  列出运行中的容器
$ docker ps -a 列出全部容器
$ docker logs 2b1b7a428627  列出标准输出
$ docker stop 2b1b7a428627 停止容器
$ docker run ubuntu:15.10 /bin/echo "Hello world"  Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。

# 运行一个web应用
$ docker pull training/webapp  # 载入镜像
$ docker run -d -P training/webapp python app.py
-d:让容器在后台运行。
-P:将容器内部使用的网络端口映射到我们使用的主机上。

# 网络端口的快捷方式
# 使用 docker port 可以查看指定 （ID或者名字）容器的某个确定端口映射到宿主机的端口号。
$ docker port 7a38a1ad55c6

# 查看应用程序日志
# docker logs [ID或者名字] 可以查看容器内部的标准输出。
# -f:让 dokcer logs 像使用 tail -f 一样来输出容器内部的标准输出。
$ docker logs -f 7a38a1ad55c6

# 查看容器的进程
$ docker top determined_swanson

# 检查程序
# 使用 docker inspect 来查看Docker的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息。
$ docker inspect determined_swanson

# 停止容器
$ docker stop determined_swanson   

# 启动容器
$ $ docker start determined_swanson

# 移除容器
$ docker rm determined_swanson  

# 列出镜像列表
$ docker images 

# 获取一个新的镜像
$ docker pull ubuntu:13.10

# 查找镜像
$  docker search httpd

# 网络端口映射
$ docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py

# docker port 命令可以让我们快捷地查看端口的绑定情况。
$ docker port adoring_stonebraker 5000

# Docker容器连接
# 端口映射并不是唯一把 docker 连接到另一个容器的方法。
# docker有一个连接系统允许将多个容器连接在一起，共享连接信息。
# docker连接会创建一个父子关系，其中父容器可以看到子容器的信息。
$ docker run -d -P --name runoob training/webapp python app.py
