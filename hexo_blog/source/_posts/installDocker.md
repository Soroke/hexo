---
title: 安装Docker和docker-compose并配置镜像加速
date: 2020-11-27 16:46:48
tags: [docker,docker安装,docker阿里云加速]
categories: [运维,docker]
---
# Docker-CE安装

## Ubuntu

### 安装必要的一些系统工具
```shell
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```

### 安装GPG证书
```shell
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

<!--more-->

### 写入软件源信息

```shell
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```
### 更新并安装 Docker-CE

```shell
sudo apt-get -y update
sudo apt-get -y install docker-ce
```

## CentOS 7

### 安装必要的一些系统工具

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
### 添加软件源信息

```shell
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
### 更新并安装 Docker-CE

```shell
sudo yum makecache fast
sudo yum -y install docker-ce
```
### 开启Docker服务

```shell
sudo service docker start
```

### 安装校验
```shell
root@iZbp12adskpuoxodbkqzjfZ:$ docker version
Client:
 Version:      17.03.0-ce
 API version:  1.26
 Go version:   go1.7.5
 Git commit:   3a232c8
 Built:        Tue Feb 28 07:52:04 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.03.0-ce
 API version:  1.26 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   3a232c8
 Built:        Tue Feb 28 07:52:04 2017
 OS/Arch:      linux/amd64
 Experimental: false
```


# docker-compose安装

## 下载程序

从github上下载docker-compose二进制文件安装

```shell
sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose
```

## 添加可执行权限 

```shell
sudo chmod +x /usr/bin/docker-compose
```
## 测试安装结果 

```shell
docker-compose --version 
```

# docker配置阿里云源
登陆阿里云镜像加速器https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors  
根据提示获取加速地址和配置加速  
检查是否生效
```shell
docker info
```
 命令返回最下面一行如过有如下内容证明配置成功  
```shell
         Registry Mirrors:
			https://xxxxxxx.mirror.aliyuncs.com/
```