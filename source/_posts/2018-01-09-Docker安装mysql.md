title: Docker安装mysql
date: 2018-01-09
categories :
  - docker
tags :
  - docker
---


## 内容：

- 安装mysql镜像
- 数据文件外部存放
- 远程连接


## 拉取mysql镜像

```sh
docker pull mysql
docker images
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysql               latest              7d83a47ab2d2        4 weeks ago         408MB
nginx               latest              40960efd7b8f        2 months ago        108MB
learn/tutorial      latest              a7876479f1aa        4 years ago         128MB
```

## 启动mysql镜像

```sh
~docker run -itd -P  mysql  bash
5445a416c761201d3d5ea03888e3c1906c2dd030b1a1272c63c2d80ee7f673eb
~ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED                  STATUS              PORTS                     NAMES
5445a416c761        mysql               "docker-entrypoint..."   Less than a second ago   Up 3 seconds        0.0.0.0:32768->3306/tcp   sleepy_ride
```

## 连接mysql镜像

```sh
docker exec -it sleepy_ride bash
```

## 检查mysql状态

```sh
~ docker exec -it sleepy_ride bash
root@5445a416c761:/#
root@5445a416c761:/#
root@5445a416c761:/# service mysql status
[info] MySQL Community Server 5.7.20 is not running.
root@5445a416c761:/# service mysql start
```

## 启动mysql

```sh
root@5445a416c761:/# service mysql status
[info] MySQL Community Server 5.7.20 is not running.
root@5445a416c761:/# service mysql start
No directory, logging in with HOME=/
2018-01-09T08:38:18.232851Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-01-09T08:38:19.854808Z 0 [Warning] InnoDB: New log files created, LSN=45790
2018-01-09T08:38:20.038572Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2018-01-09T08:38:20.128724Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 72274778-f518-11e7-a5d1-0242ac110002.
2018-01-09T08:38:20.134105Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2018-01-09T08:38:20.137243Z 1 [Warning] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
2018-01-09T08:38:21.414380Z 1 [Warning] 'user' entry 'root@localhost' ignored in --skip-name-resolve mode.
2018-01-09T08:38:21.415007Z 1 [Warning] 'user' entry 'mysql.session@localhost' ignored in --skip-name-resolve mode.
2018-01-09T08:38:21.415796Z 1 [Warning] 'user' entry 'mysql.sys@localhost' ignored in --skip-name-resolve mode.
2018-01-09T08:38:21.416317Z 1 [Warning] 'db' entry 'performance_schema mysql.session@localhost' ignored in --skip-name-resolve mode.
2018-01-09T08:38:21.416827Z 1 [Warning] 'db' entry 'sys mysql.sys@localhost' ignored in --skip-name-resolve mode.
2018-01-09T08:38:21.417310Z 1 [Warning] 'proxies_priv' entry '@ root@localhost' ignored in --skip-name-resolve mode.
2018-01-09T08:38:21.417746Z 1 [Warning] 'tables_priv' entry 'user mysql.session@localhost' ignored in --skip-name-resolve mode.
2018-01-09T08:38:21.418001Z 1 [Warning] 'tables_priv' entry 'sys_config mysql.sys@localhost' ignored in --skip-name-resolve mode.
No directory, logging in with HOME=/
..
[info] MySQL Community Server 5.7.20 is started.
```

## 进入mysql命令行

root@5445a416c761:/# mysql -u root -p

## 新建数据库、表

```sql
mysql> create database test;


mysql> CREATE TABLE Persons
    -> (
    -> Id_P int,
    -> LastName varchar(255),
    -> FirstName varchar(255),
    -> Address varchar(255),
    -> City varchar(255)
    -> );
Query OK, 0 rows affected (0.03 sec)
```


## 退出mysql镜像


## 配置远程连接

```sql
grant all privileges on *.* to 'root'@'%' identified by '123456';   这个必须有
update `mysql`.`user` set `Grant_priv` = 'Y' where `user` = 'root';
```

不能用127.0.0.1来登陆，而是要使用 ip，（20170112更正,可以用127.0.0.1）

## 验证数据

### 重启docker

docker ps ，发现运行的mysql已经关闭
docker run -itd -P  mysql  bash 后再使用docker ps 发现NAME已经变化
检查mysql服务，发现是notrunning

重启后，之前建立的数据库和表已经消失:  


`错误，是因为使用：~docker run -itd -P  mysql  bash的时候是新建一个容器，而不是启动原来的容器，使用docker ps -a 可以看到原来的容器还是在那里，只是状态是stop`



## 关闭容器

在之前已经新建了一个文件夹test

如果容器关闭、重启 数据文件是否还存在？


angry_kepler


## 挂载外部数据到容器

因为我的电脑的版本是5.6的，还不支持json查询，而公司的mysql版本是5.7，有的地方使用到了json，导致某些情况下，查询出错。升级方式如下：
https://www.jianshu.com/p/fd3aae701db9
https://segmentfault.com/a/1190000009497926

不过打算用docker  将目录挂载到docker的mysql下。



### 删除所有容器

docker rm $(docker ps -a -q)


### 新建容器

```sh
~ docker run -itd  -P  --name="docker-mysql"  mysql  bash

docker run -itd -e MYSQL_ROOT_PASSWORD=123456 --name docker-mysql -v /usr/local/mysql/data/:/var/lib/mysql -p 32774:3306 mysql
```

需要-P，否则是：

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
1ca62a63c956        mysql               "docker-entrypoint..."   4 minutes ago       Up 4 minutes        3306/tcp            docker-mysql

加-P之后

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                     NAMES
57bb2dbd5b7a        mysql               "docker-entrypoint..."   2 seconds ago       Up 3 seconds        0.0.0.0:32774->3306/tcp   docker-mysql


### 重启启动

docker start    --name="docker-mysql"  mysql  bash


docker run -itd -e MYSQL_ROOT_PASSWORD=123456 --name docker-mysql -v /usr/local/mysql/data:/var/lib/mysql -p 32774:3306 mysql

有权限问题




发现status是 Exited，  查看日志

```sh
docker logs  382edb9ddf4d

chown: cannot read directory '/var/lib/mysql/ibalife_busi': Permission denied
chown: cannot read directory '/var/lib/mysql/TIPASK': Permission denied
chown: cannot read directory '/var/lib/mysql/shopxx': Permission denied
chown: cannot read directory '/var/lib/mysql/ibalife_admin': Permission denied
chown: cannot read directory '/var/lib/mysql/ibalife_report': Permission denied
```



暂时不知道怎么解决


使用另外一个地方：

docker run -itd -e MYSQL_ROOT_PASSWORD=123456 --name docker-mysql -v /Users/liuyiyou/docker/mysql/data:/var/lib/mysql -p 32774:3306 mysql


重命名：

## 参考资料
http://blog.csdn.net/minicto/article/details/73484432

http://xiaoqiangge.com/aritcle/1496481502190.html

http://blog.csdn.net/jinzhencs/article/details/51546409
