---
title: Docker+Nginx+Tomcat 搭建负载均衡集群
date: 2018-05-5 10:22:13
categories:
    - Linux
tags:
	- Linux
---
## 一.简介
Nginx是常用的web服务器，用于获取静态资源，也可以作为负载均衡服务器。<br/>
Tomcat是常用的Servlet容器。<br/>
一般的web服务架构：Nginx作为前端服务器，Tomcat作为后端服务器。用户访问Nginx，静态资源nginx直接返回，动态资源的请求被nginx转发到tomcat，tomcat将处理完的结构返回给nginx，然后到浏览器。<br/>
而Docker作为一种容器引擎，方便我们快速搭建各种应用服务。本文将以Docker容器为基础，实现负载均衡集群。
<!--more-->
## 二.步骤
### ①.在多个Tomcat容器上部署应用
    创建Tomcat容器
```
     # 将宿主机8080端口映射到容器8080端口并链接mysql容器
     docker run -d -p 8080:8080 --name mytomcat --link mymysql tomcat
     
     docker exec -it mytomcat bash
    
     # 查看容器ip
     cat /etc/hosts
    
     cd /usr/local/tomcat/webapps
    
    # 删除Tomcat容器里的ROOT文件夹
     rm -rf ROOT
    
     exit
    
    # 为了方便，重命名应用名为ROOT.war并拷贝到容器webapps目录下，tomcat会自动部署应用到根URL路径
     docker cp ROOT.war mytomcat:/usr/local/tomcat/webapps/ROOT.war
```
     根据上述步骤，再创建一个Tomcat容器并部署应用,宿主机端口可映射为8081
### ②. 准备搭建Nginx服务
​```
    # 创建mynginx目录并进入该目录
    cd /usr/local/docker/mynginx
    
    # 创建www,log,conf目录并创建文件
    tree 
    
    ├── conf
    │   └── nginx.conf
    ├── log
    │   ├── access.log
    │   └── error.log
    └── www
        └── index.html
```
    
    修改nginx.conf为如下内容
    
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
    #
    
    #服务器集群  
    upstream  netitcast.com {  #服务器集群名字   
        server    172.17.0.3:8080  weight=1;
        server    172.17.0.4:8080  weight=2;  
    }     
      
    #当前的Nginx的配置  
    server {  
        listen       80;#监听80端口，可以改成其他端口  
        server_name  localhost;#   当前服务的域名  
      
    location / {  
            proxy_pass http://netitcast.com;  
            proxy_redirect default;  
        }  
          
    }
    }
```
### ③.搭建Nginx服务

 创建容器
```
    docker run -p 8093:80 --name mynginx  -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf -v $PWD/www:/opt/nginx/www -v $PWD/log:/opt/nginx/log  -d nginx

     命令说明：
     -p 8093:80：将容器的80端口映射到主机的8093端口
     -name mynginx：将容器命名为mynginx
     -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf：将主机中当前目录下的nginx.conf挂载到容器的/etc/nginx/nginx.conf
     -v $PWD/www:/opt/nginx/www：将主机中当前目录下的www挂载到容器的/opt/nginx/www，参考nginx.conf的root配置
     -v $PWD/log:/opt/nginx/log：将主机中当前目录下的log挂载到容器的/opt/nginx/log，参考nginx.conf的log配置
```
### ④.查看结果
打开http://localhost,可以发现nginx已经将http请求发送到172.17.0.3:8080或172.17.0.4:8080后端tomcat服务器。

