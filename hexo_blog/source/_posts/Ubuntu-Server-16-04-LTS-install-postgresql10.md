---
title: Ubuntu Server 16.04 LTS 安装 postgresql10
date: 2021-01-06 14:01:28
tags: [postgresql,ubuntu16.04]
categories: linux
---

# 添加密匙

``` shell
wget -q -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

# 添加安装源

``` shell
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

# 更新apt

``` shell
apt update
```

<!--more-->

# 安装

``` shell
lwk@qwfys ~ $ apt install postgresql-10 -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  bbswitch-dkms lib32gcc1 libc6-i386 libjansson4 libvdpau1 libxnvctrl0 screen-resolution-extra xserver-xorg-legacy
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  libjs-underscore libpq5 libwxbase3.0-0v5 libwxgtk3.0-0v5 pgadmin3-data pgdg-keyring postgresql-client-10 postgresql-client-common postgresql-common
Suggested packages:
  javascript-common postgresql-contrib locales-all postgresql-doc-10 libjson-perl
Recommended packages:
  pgagent sysstat
The following NEW packages will be installed:
  libjs-underscore libwxbase3.0-0v5 libwxgtk3.0-0v5 pgadmin3 pgadmin3-data pgdg-keyring postgresql-10 postgresql-client-10 postgresql-client-common postgresql-common
The following packages will be upgraded:
  libpq5
1 upgraded, 10 newly installed, 0 to remove and 1 not upgraded.
Need to get 17.3 MB of archives.
After this operation, 70.0 MB of additional disk space will be used.
Get:1 http://mirrors.ustc.edu.cn/ubuntu xenial/main amd64 libjs-underscore all 1.7.0~dfsg-1ubuntu1 [46.7 kB]
Get:2 http://mirrors.ustc.edu.cn/ubuntu xenial-updates/universe amd64 libwxbase3.0-0v5 amd64 3.0.2+dfsg-1.3ubuntu0.1 [971 kB]
Get:3 http://mirrors.ustc.edu.cn/ubuntu xenial-updates/universe amd64 libwxgtk3.0-0v5 amd64 3.0.2+dfsg-1.3ubuntu0.1 [4,344 kB]
Get:4 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 libpq5 amd64 10.1-1.pgdg16.04+1 [157 kB]
Get:5 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 pgadmin3-data all 1.22.2-2.pgdg16.04+1 [2,516 kB]
Get:6 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 pgadmin3 amd64 1.22.2-2.pgdg16.04+1 [3,067 kB]                                                                                                                          
Get:7 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 pgdg-keyring all 2017.3 [10.3 kB]                                                                                                                                       
Get:8 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 postgresql-client-common all 188.pgdg16.04+1 [81.5 kB]                                                                                                                  
Get:9 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 postgresql-client-10 amd64 10.1-1.pgdg16.04+1 [1,277 kB]                                                                                                                
Get:10 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 postgresql-common all 188.pgdg16.04+1 [220 kB]                                                                                                                         
Get:11 http://apt.postgresql.org/pub/repos/apt xenial-pgdg/main amd64 postgresql-10 amd64 10.1-1.pgdg16.04+1 [4,635 kB]                                                                                                                      
Fetched 17.3 MB in 6min 12s (46.5 kB/s)                                                                                                                                                                                                      
Preconfiguring packages ...
Selecting previously unselected package libjs-underscore.
(Reading database ... 210818 files and directories currently installed.)
Preparing to unpack .../libjs-underscore_1.7.0~dfsg-1ubuntu1_all.deb ...
Unpacking libjs-underscore (1.7.0~dfsg-1ubuntu1) ...
Preparing to unpack .../libpq5_10.1-1.pgdg16.04+1_amd64.deb ...
Unpacking libpq5:amd64 (10.1-1.pgdg16.04+1) over (9.5.10-0ubuntu0.16.04) ...
Selecting previously unselected package libwxbase3.0-0v5:amd64.
Preparing to unpack .../libwxbase3.0-0v5_3.0.2+dfsg-1.3ubuntu0.1_amd64.deb ...
Unpacking libwxbase3.0-0v5:amd64 (3.0.2+dfsg-1.3ubuntu0.1) ...
Selecting previously unselected package libwxgtk3.0-0v5:amd64.
Preparing to unpack .../libwxgtk3.0-0v5_3.0.2+dfsg-1.3ubuntu0.1_amd64.deb ...
Unpacking libwxgtk3.0-0v5:amd64 (3.0.2+dfsg-1.3ubuntu0.1) ...
Selecting previously unselected package pgadmin3-data.
Preparing to unpack .../pgadmin3-data_1.22.2-2.pgdg16.04+1_all.deb ...
Unpacking pgadmin3-data (1.22.2-2.pgdg16.04+1) ...
Selecting previously unselected package pgadmin3.
Preparing to unpack .../pgadmin3_1.22.2-2.pgdg16.04+1_amd64.deb ...
Unpacking pgadmin3 (1.22.2-2.pgdg16.04+1) ...
Selecting previously unselected package pgdg-keyring.
Preparing to unpack .../pgdg-keyring_2017.3_all.deb ...
Unpacking pgdg-keyring (2017.3) ...
Selecting previously unselected package postgresql-client-common.
Preparing to unpack .../postgresql-client-common_188.pgdg16.04+1_all.deb ...
Unpacking postgresql-client-common (188.pgdg16.04+1) ...
Selecting previously unselected package postgresql-client-10.
Preparing to unpack .../postgresql-client-10_10.1-1.pgdg16.04+1_amd64.deb ...
Unpacking postgresql-client-10 (10.1-1.pgdg16.04+1) ...
Selecting previously unselected package postgresql-common.
Preparing to unpack .../postgresql-common_188.pgdg16.04+1_all.deb ...
Adding 'diversion of /usr/bin/pg_config to /usr/bin/pg_config.libpq-dev by postgresql-common'
Unpacking postgresql-common (188.pgdg16.04+1) ...
Selecting previously unselected package postgresql-10.
Preparing to unpack .../postgresql-10_10.1-1.pgdg16.04+1_amd64.deb ...
Unpacking postgresql-10 (10.1-1.pgdg16.04+1) ...
Processing triggers for libc-bin (2.23-0ubuntu9) ...
Processing triggers for doc-base (0.10.7) ...
Processing 1 added doc-base file...
Registering documents with scrollkeeper...
Processing triggers for gnome-menus (3.13.3-6ubuntu3.1) ...
Processing triggers for desktop-file-utils (0.22+linuxmint1) ...
Processing triggers for mime-support (3.59ubuntu1) ...
Processing triggers for man-db (2.7.5-1) ...
Processing triggers for systemd (229-4ubuntu21) ...
Processing triggers for ureadahead (0.100.0-19) ...
ureadahead will be reprofiled on next reboot
Setting up libjs-underscore (1.7.0~dfsg-1ubuntu1) ...
Setting up libpq5:amd64 (10.1-1.pgdg16.04+1) ...
Setting up libwxbase3.0-0v5:amd64 (3.0.2+dfsg-1.3ubuntu0.1) ...
Setting up libwxgtk3.0-0v5:amd64 (3.0.2+dfsg-1.3ubuntu0.1) ...
Setting up pgadmin3-data (1.22.2-2.pgdg16.04+1) ...
Setting up pgadmin3 (1.22.2-2.pgdg16.04+1) ...
Setting up pgdg-keyring (2017.3) ...
Removing apt.postgresql.org key from trusted.gpg: OK
Setting up postgresql-client-common (188.pgdg16.04+1) ...
Setting up postgresql-client-10 (10.1-1.pgdg16.04+1) ...
update-alternatives: using /usr/share/postgresql/10/man/man1/psql.1.gz to provide /usr/share/man/man1/psql.1.gz (psql.1.gz) in auto mode
Setting up postgresql-common (188.pgdg16.04+1) ...
Adding user postgres to group ssl-cert

Creating config file /etc/postgresql-common/createcluster.conf with new version
Building PostgreSQL dictionaries from installed myspell/hunspell packages...
  en_us
Removing obsolete dictionary files:
Setting up postgresql-10 (10.1-1.pgdg16.04+1) ...
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Unescaped left brace in regex is deprecated, passed through in regex; marked by <-- HERE in m/(?<!\\)\${ <-- HERE ([^}]+)}/ at /usr/sbin/pam_getenv line 78.
Creating new PostgreSQL cluster 10/main ...
/usr/lib/postgresql/10/bin/initdb -D /var/lib/postgresql/10/main --auth-local peer --auth-host md5
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locales
  COLLATE:  en_US.UTF-8
  CTYPE:    en_US.UTF-8
  MESSAGES: en_US.UTF-8
  MONETARY: zh_CN.UTF-8
  NUMERIC:  zh_CN.UTF-8
  TIME:     en_US.UTF-8
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/10/main ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

Success. You can now start the database server using:

    /usr/lib/postgresql/10/bin/pg_ctl -D /var/lib/postgresql/10/main -l logfile start

Ver Cluster Port Status Owner    Data directory              Log file
10  main    5432 down   postgres /var/lib/postgresql/10/main /var/log/postgresql/postgresql-10-main.log
update-alternatives: using /usr/share/postgresql/10/man/man1/postmaster.1.gz to provide /usr/share/man/man1/postmaster.1.gz (postmaster.1.gz) in auto mode
Processing triggers for libc-bin (2.23-0ubuntu9) ...
Processing triggers for systemd (229-4ubuntu21) ...
Processing triggers for ureadahead (0.100.0-19) ...
```

# 使用方法

### 启动|关闭|重启

``` shell
/etc/init.d/postgresql start|stop|restart
```

### 使用postgres用户登录到数据库

``` shell
sudo -u postgres psql
```

### 切换数据库

``` shell
\c dbname   --dbanme是数据库名称
```

### 查看数据库所有者

``` shell
\l
```

### 查看所有用户的角色

``` shell
\du
```

### 创建用户

``` shell
create user username with password '***';    --username用户名，***用户密码
```

### 创建数据库并指定所有者

``` shell
create database dbname owner username;    --dbname数据库名，username用户名
```

### 更改数据库的所有者

``` shell
grant all on database dbname to username;   --dbname 数据库名称，username用户名称
```

# 为数据库创建只读用户

### 创建只读用户dev

``` shell
CREATE USER dev WITH ENCRYPTED PASSWORD '****';
```

### 设置dev默认事务为只读

``` shell
alter user dev set default_transaction_read_only=on;
```

### 赋予用户dev连接指定数据库bot的权限

``` shell
grant connect on database bot to dev;
```

### 切换到数据库bot

``` shell
\c bot
```

### 把数据库bot所有在public这个schema下的所有表的使用权授予dev用户

``` shell
grant usage on schema public to dev;
```

### 将之后新建在public这个schema下的所有表的使用权授予dev用户

``` shell
alter default privileges in schema public grant select on tables to dev;
```

### 赋予用户dev所有public下的序列查询权限

``` shell
grant usage on all sequences in schema public to dev;
```

### 赋予用户dev所有public下表的select权限

``` shell
grant select on all tables in schema public to dev;
```

# 数据库重新导入表后的授权

### 使用postgres用户登录到数据库

``` shell
sudo -u postgres psql
```

### 更改数据库bot的所有者为bot用户

``` shell
grant all on database bot to bot;   --dbname 数据库名称，username用户名称
```

### 赋予用户bot连接指定数据库bot的权限

``` shell
grant connect on database bot to bot;
```

### 赋予用户dev连接指定数据库bot的权限

``` shell
grant connect on database bot to dev;
```

### 切换到数据库bot

``` shell
\c bot
```

### 把数据库bot所有在public这个schema下的所有表的使用权授予bot用户

``` shell
grant usage on schema public to bot;
```

### 赋予用户bot所有public下的所有权限

``` shell
grant usage on all sequences in schema public to bot;
```

### 赋予用户bot所有public下表的所有权限

``` shell
grant all on all tables in schema public to bot;
```

### 把数据库bot所有在public这个schema下的所有表的使用权授予dev用户

``` shell
grant usage on schema public to dev;
```

### 将之后新建在public这个schema下的所有表的使用权授予dev用户

``` shell
alter defaule privileges in schema public grant  select on tables to dev;
```
### 赋予用户dev所有public下的序列查询权限

``` shell
grant usage on all sequences in schema public to dev;
```

### 赋予用户dev所有public下表的select权限

``` shell
grant select on all tables in schema public to dev;
```