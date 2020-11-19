---
title: 使用docker和docker-compose启动Jenkins
date: 2020-11-18 22:04:55
tags: Jenkins
categories: 运维
---

## 基础环境

所需软件如下
```
jdk
docker
docker-compose
```

## yml文件配置

```
jenkins:
    image: jenkins/jenkins:lts
    volumes:
        - ./jenkins_home/:/var/jenkins_home
        - /var/run/docker.sock:/var/run/docker.sock
        - /usr/bin/docker:/usr/bin/docker
        - /usr/lib64/libltdl.so.7:/usr/lib64/libltdl.so.7
        - /usr/local/bin/docker-compose:/usr/local/bin/docker-compose
    ports:
        - "80:8080"
        - "50000:50000"
    privileged: true
    user: root
    restart: always
    container_name: Jenkins
```



## 启动jenkins

 将第二步中的yaml文件放在指定目录下，然后进入该文件夹，启动服务
```sh
$ docker-compose up -d
```

## 访问并安装插件
访问本机http://localhost:80

首次访问需要密码，查看默认生成密码：

```sh
$ cat jenkins_home/secrets/initialAdminPassword
```

## 解决问题
### 一直卡在启动页面
修改文件：
```sh
$ vim jenkins_home/hudson.model.UpdateCenter.xml
```
将https://updates.jenkins.io/update-center.json

修改为 https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json

然后重启服务

---
### Jenkins离线
修改文件
```sh
$ vim jenkins_home/updates/default.json
```
修改第一行的”connectionCheckUrl”:http://www.google.com/“

改为：”connectionCheckUrl”:http://www.baidu.com/“

然后重启服务

---
### 安装完成后，空白页面
重启服务
```sh
$ docker-compose restart
```