---
title: centos7 nimi通过添加yum源安装mysql
date: 2021-01-06 13:45:44
tags: [mysql,centos7]
categories: linux
---

# 配置yum源

### 在MySQL官网中下载YUM源rpm安装包：[官网获取rpm包的yum源](http://dev.mysql.com/downloads/repo/yum/)

``` shell
# 下载mysql源安装包
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
# 安装mysql源
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```

### 检查mysql源是否安装成功

``` shell
yum repolist enabled | grep "mysql.*-community.*"
```

# 安装Mysql

### yum安装

``` shell
yum install mysql-community-server
```

### 启动mysql

``` shell
systemctl start mysqld
```

<!--more-->


### 查看MySQL的启动状态

``` shell
systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; disabled; vendor preset: disabled)
   Active: active (running) since 五 2016-06-24 04:37:37 CST; 35min ago
 Main PID: 2888 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─2888 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

6月 24 04:37:36 localhost.localdomain systemd[1]: Starting MySQL Server...
6月 24 04:37:37 localhost.localdomain systemd[1]: Started MySQL Server.
```

### 设置开机启动

``` shell
systemctl enable mysqld
systemctl daemon-reload
```

# 配置mysql

### 查找初始root密码

此时MySQL已经开始正常运行，不过要想进入MySQL还得先找出此时root用户的密码，通过如下命令可以在日志文件中找出密码

``` shell
grep "password" /var/log/mysqld.log
```

### 重置root密码

``` shell
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
```

### 修改root默认密码,开启root用户远程访问权限

``` shell
mysql -uroot -p
use mysql;
update user set Password=PASSWORD('new password') where User='root';
GRANT ALL PRIVILEGES ON *.* TO root@"%.%.%.%" IDENTIFIED BY "new password";
flush privileges;
exit;
```

### 配置默认编码为utf8

修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置

```
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
```
然后重启mysql服务
