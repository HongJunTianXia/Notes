## 宿主机准备
```
root@c-pc:/opt/docker_nginx# ls
conf  log  www
```
conf存放nginx的配置文件，log存放nginx的日志，www存放模板文件
```
root@c-pc:/opt/docker_nginx# tree 
.
├── conf
│   └── nginx.conf
├── log
│   ├── access.log
│   └── error.log
└── www
    └── index.html

```
以下是nginx.conf内容
```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 768;
    # multi_accept on;
}

http {

    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /opt/nginx/log/access.log;
    error_log /opt/nginx/log/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;
    gzip_disable "msie6";

    ##
    # Virtual Host Configs
    ##

    server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /opt/nginx/www;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
            try_files $uri $uri/ =404;
        }

    }
}
```
以上这个配置从nginx的默认配置文件复制出来的，改动log、root相关内容，即：
```
...
access_log /opt/nginx/log/access.log;
error_log /opt/nginx/log/error.log;
...
root /opt/nginx/www;
...
```
index.html
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h2>Hello docker nginx</h1>
</body>
</html>
```

## 启动容器
```
root@c-pc:/opt/docker_nginx# docker run -p 8093:80 --name mynginx  -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf -v $PWD/www:/opt/nginx/www -v $PWD/log:/opt/nginx/log  -d nginx
026f7dc3dfa4362210a5273775e01222c6b622a9298ec20e9d53f46d9bc72f20
```
命令说明：
```
 -p 8093:80：将容器的80端口映射到主机的8093端口
  -name mynginx：将容器命名为mynginx
  -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf：将主机中当前目录下的nginx.conf挂载到容器的/etc/nginx/nginx.conf
  -v $PWD/www:/opt/nginx/www：将主机中当前目录下的www挂载到容器的/opt/nginx/www，参考nginx.conf的root配置
  -v $PWD/log:/opt/nginx/log：将主机中当前目录下的log挂载到容器的/opt/nginx/log，参考nginx.conf的log配置
```
