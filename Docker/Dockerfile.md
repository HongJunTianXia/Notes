使用Dockerfile构建镜像并上传
```
docker rmi -f $(docker images -q -f reference=imagetools:latest)
docker build -t imagetools:latest .
docker tag imagetools:latest registry.cn-beijing.aliyuncs.com/******/imagetools:v2.0.5
docker push registry.cn-beijing.aliyuncs.com/******/imagetools:v2.0.5
```

登陆docker仓库
docker login -u ***@***.com -p ****** registry.cn-beijing.aliyuncs.com/******
