---
title: docker镜像web在线管理工具-portainer
date: 2020-11-27 16:13:22
tags: [docker,portainer,portainer汉化,docker-web管理]
categories: [运维,docker]
---

# portainer git地址
[https://github.com/portainer/portainer](https://github.com/portainer/portainer)

# 所需运行环境

所需软件名称|版本号
:----:|:----:
docker|v17.06.0-ce及以上
docker-compose|v1.18.0及以上

<!--more-->

# 流程

### 下载portainer汉化包

创建运行目录，并下载中文包
```shell
$ cd /opt && mkdir portainer-cn
$ cd portainer-cn
$ wget https://dl.quchao.net/Soft/Portainer-CN.zip -O Portainer-CN.zip
$ unzip -d ./Portainer-CN Portainer-CN.zip
```

### 创建启动docker-compose.yml

```shell
$ cd /opt/portainer-cn
$ touch docker-compose.yml
```
文件内容如下

```
version: '3'
services:
  portainer:
    image: portainer/portainer:latest
    ports:
    - "9001:9000" #9001为提供服务端口,按需修改
    volumes:
    - /var/run/docker.sock:/var/run/docker.sock
    - ./Portainer-CN:/public #映射汉化包进去
    restart: always
```

### 运行

```shell
$ docker-compose up -d
```