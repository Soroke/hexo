---
title: ShadowsocksR+Caddy+TLS伪装流量科学上网
date: 2020-12-03 10:21:48
tags: [ShadowsocksR,科学上网]
categories: [科学上网]
---

# 简介

无论是ss还是ssr最近都出现了IP频繁被墙的情况，本文介绍如何通过ShadowsocksR+Caddy+TLS伪装流量科学上网，大大减小VPS被墙的概率。

# 购买vps
- [免费注册Google Cloud](https://console.cloud.google.com/)

- [点击优惠注册vultr](https://www.vultr.com/?ref=7931367)


<!--more-->

# 进入管理员(root)
- 需要进入root才能安装各种各样的服务
```mac
sudo su
```

# 开启Google BBR加速(可选)
Linux内核4.9以及上默认支持了该算法，直接启用就行(谷歌云)  

- 检查Linux内核版本
```shell
uname -r
```

- 添加并启用Google BBR模块到内核
```shell
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

- 检查是否成功加入Google BBR模块到内核中
```shell
sysctl net.ipv4.tcp_available_congestion_control
```

- 验证是否启用了Google BBR
```shell
lsmod | grep bbr
```

# 在线工具
- [在线文本编辑器](https://www.editpad.org/)

# 配置caddy服务器

- 安装caddy

```shell
curl https://getcaddy.com | bash -s personal
```

- 查看caddy所在位置

```shell
whereis caddy
```

- 创建网站目录

```shell
mkdir -p /usr/local/caddy/www/ && cd /usr/local/caddy/www/
```

- 创建网页测试文件

```shell
echo "Welcome to subscribe to my YouTube channel." > index.html
```

- 运行caddy服务器

```shell
caddy
```

- 访问caddy服务器(默认端口:2015)
http://example_ip:2015

- CTRL+ C 停止caddy运行
- 下载网页模版并解压到/usr/local/caddy/www/目录

+ [网页模板1](https://www.free-css.com/free-css-templates)
+ [网页模板2](https://colorlib.com/wp/templates/)

- 安装unzip

```shell
Centos
yum install wget unzip -y

Debian/Ubuntu
apt install wget unzip -y
```

- 下载模板

```shell
wget website.zip # 下载文件
ls # 查看当前文件夹的文件，看是否包含下载的网站文件
unzip website.zip # 解压文件
mv website/* . # 如果解压后项目文件包含在文件夹需要移动解压后的项目文件到当前目录
```

- 创建Caddyfile

```shell
echo "https://v2ray.duyuanchao.me:2333 {
root /usr/local/caddy/www/
timeouts none
tls duyuanchao.me@gmail.com
}" > /usr/local/caddy/Caddyfile
```

- 再次运行caddy服务器测试

```shell
cd /usr/local/caddy/ # 必须切换到Caddyfile所在目录
caddy
```

- https访问网站

https://v2ray.duyuanchao.me:2333

- Ctrl + C停止caddy

- 后台运行caddy服务

```shell
nohup caddy &
```

- 停止caddy

```shell
ps -ax | grep caddy
kill `caddy-id`
```

# 开启https端口号(443)

这里强力建议不要直接把防火墙关闭暴露所有端口号，而是打开防火墙减少服务器被墙的风险。

- 安装ufw
>UFW 全称为 Uncomplicated Firewall，是 Ubuntu 系统上默认的防火墙组件, 为了轻量化配置 iptables 而开发的一款工具。UFW 提供一个非常友好的界面用于创建基于IPV4，IPV6的防火墙规则。

Centos|Debian/Ubuntu
:-----:|:-----:|:-----:
```yum -y install ufw```|```apt install ufw -y```

- 查看防火墙状态

```shell
ufw status
```

- 开启防火墙

```shell
ufw enable
```

- 关闭防火墙

```shell
ufw disable
```

- 开启https端口(443)

```shell
ufw allow 443,2333/tcp
```

- 开启ssh端口

```shell
ufw allow ssh
```

# 配置ShadowsocksR

- 安装ShadowsocksR

```shell
wget -N --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh && chmod +x shadowsocksR.sh && ./shadowsocksR.sh
```

- 替换SSR配置文件

```shell
cat <<EOT > /etc/shadowsocks.json
{
    "server":"0.0.0.0",
    "server_ipv6":"[::]",
    "server_port":443,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"duyuanchao",
    "timeout":120,
    "method":"none",
    "protocol":"auth_chain_a",
    "protocol_param":"",
    "obfs":"tls1.2_ticket_auth",
    "obfs_param":"",
    "redirect":["*:443#127.0.0.1:2333"],
    "dns_ipv6":false,
    "fast_open":false,
    "workers":1
}
EOT
```

- 重启ShadowsocksR

```shell
/etc/init.d/shadowsocks restart
```

## 	客服端链接
客服端切记端口号为443,加密方式为none

```shell
ip: v2ray.duyuanchao.me
端口号: 443
密码: duyuanchao
加密: none
协议: auth_chain_a
混淆: tls1.2_ticket_auth
```