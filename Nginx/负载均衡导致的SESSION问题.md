### 方法一 添加负载均衡为ip_hash
```
upstream nginx.example.com
{
    server 127.0.0.1:8080;
    server 127.0.0.1:808;
    ip_hash;
}
server
{
    listen 80;
    location /
    {
        proxy_pass
        http://nginx.example.com;
    }
}
```
### 方法二 session存在memcache或者redis中
