---
title: Harbor平台搭建安装
date: 2020-11-19 21:02:39
tags: [docker,harbor]
categories: [运维,docker]
---
# 硬件要求
硬件资源|最低配置|推荐配置
:-----|:-----:|:-----:
处理器|2个CPU|4个CPU
内存|4GB|8GB
磁盘|40GB|160GB

# 软件要求

软件资源|版本|
:-----|:-----:
Docker engine|v17.06.0-ce及以上
Docker compose|v1.18.0及以上
openssl|尽量使用最新版本
<!--more-->
# 网络端口

端口|协议|
:-----|:-----:
443|https
4443|https
80|http

---
# 下载地址

Harbor官方分别提供了在线版（不含组件镜像，相对较小）和离线版（包含组件镜像，相对较大）。

下载offline包就行
```
https://github.com/goharbor/harbor/releases
```
---
# 创建自签名 https 证书

## 生成证书

### 创建证书目录，并赋予权限
```shell
$ mkdir -p /data/cert && chmod -R 777 /data/cert && cd /data/cert
```

### 创建CA根证书
```shell
$ openssl req  -newkey rsa:4096 -nodes -sha256 -keyout ca.key -x509 -days 365 -out ca.crt -subj "/C=CN/L=zhejiang/O=lisea/CN=harbor-registry"
```
### 生成一个证书签名, 假设设置访问域名为harbor.faxuan.net(自行替换域名)
```shell
$ openssl req -newkey rsa:4096 -nodes -sha256 -keyout harbor.faxuan.net.key -out server.csr -subj "/C=CN/L=zhejiang/O=lisea/CN=harbor.faxuan.net"
```
### 生成主机的证书(自行替换域名)
```shell
$ openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out harbor.faxuan.net.crt
```

## 创建客户端使用证书
```shell
$ mkdir harbor.faxuan.net
$ cp ca.crt harbor.faxuan.net && cp harbor.faxuan.net.key harbor.faxuan.net && cp harbor.faxuan.net.crt harbor.faxuan.net/harbor.faxuan.net.cert
$ tar -czvf harbor.faxuan.net.tar.gz harbor.faxuan.net
```
执行完成后在当前目录下会生成一个harbor.faxuan.net.tar.gz文件，这个文件就是客户端需要使用的文件

---
# 部署安装
## 解压软件包
```shell
$ tar -zxvf harbor-offline-installer-v2.1.0.tar.gz
```

## 修改配置
```shell
$ cd harbor
$ vim harbor.yml
```
修改如下配置  
hostname: harbor.faxuan.net  **修改为自己的域名**  
http:  
  port: 80  
  
https:  
  port: 443  
  certificate: /data/cert/harbor.faxuan.net.crt  **修改为自己的路径以及证书**  
  private_key: /data/cert/harbor.faxuan.net.key  **修改为自己的路径以及证书**  
  
harbor_admin_password: Fxzx1234  **管理员密码**

## 执行安装
```shell
$ ./install.sh
```

```
[root@hub harbor]# ./install.sh

[Step 0]: checking if docker is installed ...
Note: docker version: 19.03.8

[Step 1]: checking docker-compose is installed ...
Note: docker-compose version: 1.23.1

[Step 2]: loading Harbor images ...
Loaded image: goharbor/clair-adapter-photon:v1.0.1-v1.10.1
Loaded image: goharbor/harbor-jobservice:v1.10.1
Loaded image: goharbor/redis-photon:v1.10.1
Loaded image: goharbor/notary-server-photon:v0.6.1-v1.10.1
Loaded image: goharbor/clair-photon:v2.1.1-v1.10.1
Loaded image: goharbor/harbor-log:v1.10.1
Loaded image: goharbor/registry-photon:v2.7.1-patch-2819-2553-v1.10.1
Loaded image: goharbor/notary-signer-photon:v0.6.1-v1.10.1
Loaded image: goharbor/chartmuseum-photon:v0.9.0-v1.10.1
Loaded image: goharbor/harbor-registryctl:v1.10.1
Loaded image: goharbor/nginx-photon:v1.10.1
Loaded image: goharbor/harbor-migrator:v1.10.1
Loaded image: goharbor/prepare:v1.10.1
Loaded image: goharbor/harbor-portal:v1.10.1
Loaded image: goharbor/harbor-core:v1.10.1
Loaded image: goharbor/harbor-db:v1.10.1

[Step 3]: preparing environment ...

[Step 4]: preparing harbor configs ...
prepare base dir is set to /root/harbor
Clearing the configuration file: /config/log/logrotate.conf
Clearing the configuration file: /config/log/rsyslog_docker.conf
Clearing the configuration file: /config/nginx/conf.d/notary.upstream.conf
Clearing the configuration file: /config/nginx/conf.d/notary.server.conf
Clearing the configuration file: /config/nginx/nginx.conf
Clearing the configuration file: /config/core/env
Clearing the configuration file: /config/core/app.conf
Clearing the configuration file: /config/registry/config.yml
Clearing the configuration file: /config/registry/root.crt
Clearing the configuration file: /config/registryctl/env
Clearing the configuration file: /config/registryctl/config.yml
Clearing the configuration file: /config/db/env
Clearing the configuration file: /config/jobservice/env
Clearing the configuration file: /config/jobservice/config.yml
Clearing the configuration file: /config/notary/server-config.postgres.json
Clearing the configuration file: /config/notary/server_env
Clearing the configuration file: /config/notary/signer_env
Clearing the configuration file: /config/notary/signer-config.postgres.json
Clearing the configuration file: /config/notary/notary-signer.crt
Clearing the configuration file: /config/notary/notary-signer.key
Clearing the configuration file: /config/notary/notary-signer-ca.crt
Clearing the configuration file: /config/notary/root.crt
Generated configuration file: /config/log/logrotate.conf
Generated configuration file: /config/log/rsyslog_docker.conf
Generated configuration file: /config/nginx/nginx.conf
Generated configuration file: /config/core/env
Generated configuration file: /config/core/app.conf
Generated configuration file: /config/registry/config.yml
Generated configuration file: /config/registryctl/env
Generated configuration file: /config/db/env
Generated configuration file: /config/jobservice/env
Generated configuration file: /config/jobservice/config.yml
loaded secret from file: /secret/keys/secretkey
Generated configuration file: /compose_location/docker-compose.yml
Clean up the input dir

# 如果已安装harbor将会自动移除
Note: stopping existing Harbor instance ...
Stopping nginx             ... done
Stopping harbor-jobservice ... done
Stopping harbor-core       ... done
Stopping redis             ... done
Stopping harbor-portal     ... done
Stopping registry          ... done
Stopping harbor-db         ... done
Stopping harbor-log        ... done
Removing nginx             ... done
Removing harbor-jobservice ... done
Removing harbor-core       ... done
Removing redis             ... done
Removing harbor-portal     ... done
Removing registry          ... done
Removing harbor-db         ... done
Removing registryctl       ... done
Removing harbor-log        ... done
Removing network harbor_harbor


[Step 5]: starting Harbor ...
Creating network "harbor_harbor" with the default driver
Creating harbor-log ... done
Creating harbor-portal ... done
Creating registry      ... done
Creating redis         ... done
Creating harbor-db     ... done
Creating registryctl   ... done
Creating harbor-core   ... done
Creating harbor-jobservice ... done
Creating nginx             ... done
✔ ----Harbor has been installed and started successfully.----
```

# 客户端配置证书并登录

## 安装证书

```
$ mkdir /etc/docker/certs.d/
$ cd /etc/docker/certs.d/
$ rz -be harbor.faxuan.net.tar.gz  上传证书压缩包到当前目录(压缩包见步骤证书生成)
$ tar -xvf harbor.faxuan.net.tar.gz
$ systemctl restart docker
```
## 配置hosts

```shell
$ echo "xx.xx.xx.xx harbor.faxuan.net" >> /etc/hosts
```

## 登录到harbor平台

```
$ docker login harbor.faxuan.net
```

## 注意事项
生成镜像时 镜像名称需要指定为 harbor.faxuan.net/项目名称/镜像名称:version