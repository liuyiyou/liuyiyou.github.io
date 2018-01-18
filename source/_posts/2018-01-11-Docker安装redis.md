title: Docker安装redis
date: 2018-01-11
categories :
  - docker
tags :
  - docker
---


## 内容：

- 直接通过官方Images安装
- 编写Dockerfile安装
- 数据文件外部存放
- 远程连接

## 直接通过官方Images安装
```
docker search redis
docker pull redis
```
## 编写Dockerfile安装
待续。。。。。。

## 数据文件外部存放
```
## 第一次
docker run -p 63790:6379  --name docker-redis -v /Users/liuyiyou/docker/redis/data:/data  -d redis redis-server
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                     NAMES
dbe780e50a3b        redis               "docker-entrypoint.s…"   5 days ago          Up 5 seconds        0.0.0.0:63790->6379/tcp   docker-redis
## 后续
docker start  docker-redis
```

命令说明：
-d  后台运行
-p 63790:6379 :将容器的6379端口映射到主机的63790端口
-v User/data:/data :将主机中当前目录下的data挂载到容器的/data
redis-server --appendonly yes :在容器执行redis-server启动命令，并打开redis持久化配置
