```
# 下载mysql镜像
$ docker pull mysql 

# 启动mysql实例
# --name 后面的是docker容器名
# -p 32xxx:3306 这里需要注意 `32xxx` 是你**链接mysql的时候的`Port`。
# -e MYSQL_ROOT_PASSWORD 是设置mysql的root账号密码
# -d mysql 是你的镜像标签
$ docker run --name mymysql -p 32xxx:3306 -e MYSQL_ROOT_PASSWORD=1234 -d mysql:latest

# 在Shell中访问MySQL
$ docker exec -it mymysql bash

# 查看日志
$ docker logs mymysql



```