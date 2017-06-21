---
title: Docker 安装MongoDB分片集群
layout: post
date: '2017-06-22 00:00:00'
categories: docker MongoDB
tags: docker MongoDB
author: luamas
original: true
---

* content
{:toc}

{% raw %}

docker下面做分片集群看似简单，其实坑还是很多的，下面总结下。






### 直接看`docker-compose.yml`文件

```bash
version: '3'
services:
  csrs1:
    image: mongo:latest
    volumes:
      - /data/mongodb/cs/rs1:/data/db
    command: mongod --auth --configsvr --replSet csrs --dbpath /data/db
  csrs2:
    image: mongo:latest
    volumes:
      - /data/mongodb/cs/rs2:/data/db
    command: mongod --auth --configsvr --replSet csrs --dbpath /data/db
  csrs3:
    image: mongo:latest
    volumes:
      - /data/mongodb/cs/rs3:/data/db
    command: mongod --auth --configsvr --replSet csrs --dbpath /data/db
  mongos:
    image: mongo:latest
    volumes:
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongos --configdb csrs/csrs1:27019,csrs2:27019,csrs3:27019
    ports:
      - "27017:27017"
  shrs1:
    image: mongo:latest
    volumes:
      - /data/mongodb/sh/rs1:/data/db
    command: mongod --auth --dbpath /data/db --shardsvr --replSet shrs
  shrs2:
    image: mongo:latest
    volumes:
      - /data/mongodb/sh/rs2:/data/db
    command: mongod --auth --dbpath /data/db --shardsvr --replSet shrs
  shrs3:
    image: mongo:latest
    volumes:
      - /data/mongodb/sh/rs3:/data/db
    command: mongod --auth --dbpath /data/db --shardsvr --replSet shrs
```


### 启动docker
```
docker-compose up -d
```

采用`docker ps`看下是否都已经启动成功

### 配置`config server`

```
docker-compose exec csrs1 mongo --port 27019
rs.initiate()
rs.add('csrs2:27019')
rs.add('csrs3:27019')
rs.status() //查看状态
exit
```

`config server` 默认端口为 27019

### 配置`shard server`

```
docker-compose exec shrs1 mongo --port 27018
rs.initiate()
var cfg = rs.conf()
cfg.members[0].host = 'shrs1:27018'
rs.reconfig(cfg)
rs.add('shrs2:27018')
rs.add('shrs3:27018')
rs.status() //查看状态
exit
```

`shard server` 默认端口号为 27018

### 配置 `mongos`

```
docker-compose exec mongos mongo
sh.addShard('shrs/shrs1:27018')
sh.status() //查看状态
exit
```

##授权模式登陆

### 创建`keyfile`文件

```
mkdir /data/mongodb/key.d
cd /data/mongodb/key.d
openssl rand -base64 741 > keyfile //确认自己是否已安装openssl，生成的字符数要求在6到1024个字符之间
chmod 600 keyfile   //不要给的权限太大，否则会入坑
sudo chown 999 keyfile  //这个一定要执行，否则还是没有权限
```

### 创建用户

我们在无授权验证模式下创建用户

```
docker-compose exec mongos mongo
use admin
db.createUser( {
 user: "test",
 pwd: "test",
 roles: [ { role: "root", db: "admin" } ]
}); //我们给到root权限，具体有什么权限可以查看官方文档
db.auth("test","test") //验证用户，返回1代表正确
exit
```

### 修改`docker-compose。yml`文件

```
version: '3'
services:
  csrs1:
    image: mongo:latest
    volumes:
      - /data/mongodb/cs/rs1:/data/db
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongod --auth --configsvr --replSet csrs --dbpath /data/db --keyFile /opt/keyfile
  csrs2:
    image: mongo:latest
    volumes:
      - /data/mongodb/cs/rs2:/data/db
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongod --auth --configsvr --replSet csrs --dbpath /data/db --keyFile /opt/keyfile
  csrs3:
    image: mongo:latest
    volumes:
      - /data/mongodb/cs/rs3:/data/db
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongod --auth --configsvr --replSet csrs --dbpath /data/db --keyFile /opt/keyfile
  mongos:
    image: mongo:latest
    volumes:
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongos --configdb csrs/csrs1:27019,csrs2:27019,csrs3:27019 --keyFile /opt/keyfile
    ports:
      - "27017:27017"
  shrs1:
    image: mongo:latest
    volumes:
      - /data/mongodb/sh/rs1:/data/db
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongod --auth --dbpath /data/db --shardsvr --replSet shrs --keyFile /opt/keyfile
  shrs2:
    image: mongo:latest
    volumes:
      - /data/mongodb/sh/rs2:/data/db
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongod --auth --dbpath /data/db --shardsvr --replSet shrs --keyFile /opt/keyfile
  shrs3:
    image: mongo:latest
    volumes:
      - /data/mongodb/sh/rs3:/data/db
      - /data/mongodb/key.d/keyfile:/opt/keyfile
    command: mongod --auth --dbpath /data/db --shardsvr --replSet shrs --keyFile /opt/keyfile
```

注意上面的mongos，它没有`--auth`。

### 我们执行下列命令即可重启docker，注意要在docker-compose下面执行`docker-compose up -d`

至此结束

{% endraw %}

