```
worker_processes  1;#工作进程的个数，一般与计算机的cpu核数一致  
  
events {  
    worker_connections  1024;#单个进程最大连接数（最大连接数=连接数*进程数）  
}  
  
http {  
    include       mime.types; #文件扩展名与文件类型映射表  
    default_type  application/octet-stream;#默认文件类型  
  
    sendfile        on;#开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。  
      
    keepalive_timeout  65; #长连接超时时间，单位是秒  
  
    gzip  on;#启用Gizp压缩  
      
    #服务器的集群  
    upstream  netitcast.com {  #服务器集群名字   
        server    127.0.0.1:18080  weight=1;#服务器配置   weight是权重的意思，权重越大，分配的概率越大。  
        server    127.0.0.1:28080  weight=2;  
    }     
  
    #当前的Nginx的配置  
    server {  
        listen       80;#监听80端口，可以改成其他端口  
        server_name  localhost;##############   当前服务的域名  
  
    location / {  
            proxy_pass http://netitcast.com;  
            proxy_redirect default;  
        }  
          
  
        error_page   500 502 503 504  /50x.html;  
        location = /50x.html {  
            root   html;  
        }  
    }  
}  
```
