---
title: 搭建私有云nextcloud
date: 2021-01-12 12:29:20
tags: ['私有云','nextcloud','docker']
categories: nas

---

# 所需环境

软件资源|版本|
:-----:|:-----:
Docker engine|v17.06.0-ce及以上
Docker compose|v1.18.0及以上
openssl|尽量使用最新版本

# 配置下载
软件名称|软件下载地址
:----:|:----:
cloud.zip|[下载地址](/lib/cloud.zip)

<!--more-->

# 使用方法

## 下载并解压配置

``` shell
unzip cloud.zip
```
## 设置初始化值

```
修改conf 文件夹下的所有文件
设置的是管理员用户名、管理员密码、数据库用户名、密码 数据库名称
```

## 启动

```
docker-compose up -d
```

## 修改配置

停止运行

```
docker-compose down
```

修改文件

```
修改文件./nextcloud/config/config.php

第三行增加配置：
'default_language' => 'zh_CN',

找到行“ 0 => 'localhost',”
在其下新增一行 1 => 'ip地址/域名',
```
重新启动

```
docker-compose up -d
```