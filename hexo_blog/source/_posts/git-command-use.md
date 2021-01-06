---
title: git命令使用
date: 2021-01-06 14:13:28
tags: [github,gitlab]
categories: git
---


# git安装后配置

## 配置用户名和邮箱

``` shell
git config --global user.name "user_test"
git config --global user.email "user_test@user.test"
```

## 远程推送问题和解决

### 问题如下

``` shell
ssh: connect to host github.com port 22: Connection refused
fatal: Could not read from remote repository.


Please make sure you have the correct access rights
and the repository exists.
```

### 解决方法

``` shell
vim .ssh/config

Host github.com
User fulinux@sina.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

<!--more-->


## 测试连接是否成功

``` shell
$ ssh -T git@github.com

Hi fulinux! You've successfully authenticated, but GitHub does not provide shell access.
```

# git冲突处理

## 先将本地修改暂存
这样本地的所有修改就都被暂存起来，用 git stash list 可以看到保存的信息：其中 stash{0} 就是刚才保存的内容

```shell
git stash
```

## pull内容
暂存了本地就可以愉快地pull了

```shell
git pull
```

## 还原暂存的内容

```shell
git stash pop stash@{0}
```

# git 放弃本地修改

``` shell
git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态
git stash #把所有没有提交的修改暂存到stash里面。可用git stash pop回复。
git reset --hard HASH #返回到某个节点，不保留修改。
git reset --soft HASH #返回到某个节点。保留修改

git checkout . && git clean -df  # 新增的文件需要删除
```


# Git分支的使用

## 远程仓库有master和dev分支

### 克隆代码

```shell
git clone https://github.com/master-dev.git
# 这个git路径是无效的，示例而已
```

### 查看所有分支

```shell
git branch --all
# 默认有了dev和master分支，所以会看到如下三个分支
# master[本地主分支] origin/master[远程主分支] origin/dev[远程开发分支]
# 新克隆下来的代码默认master和origin/master是关联的，也就是他们的代码保持同步
# 但是origin/dev分支在本地没有任何的关联，所以我们无法在那里开发
```

### 创建本地关联origin/dev的分支

```shell
git checkout dev origin/dev
# 创建本地分支dev，并且和远程origin/dev分支关联，本地dev分支的初始代码和远程的dev分支代码一样
```

### 切换到dev分支进行开发

```shell
git checkout dev
# 这个是切换到dev分支，然后就是常规的开发
```

## 假设远程仓库只有mater分支

### 创建本地新的dev分支

```shell
git branch dev
# 创建本地分支 git branch
# 查看分支
# 这是会看到master和dev，而且master上会有一个星号
# 这个时候dev是一个本地分支，远程仓库不知道它的存在
# 本地分支可以不同步到远程仓库，我们可以在dev开发，然后merge到master，使用master同步代码，当然也可以同步
```

### 发布dev分支

```shell
git push origin dev:dev # 这样远程仓库也有一个dev分支了
```

### 在dev分支开发代码

```shell
git checkout dev # 切换到dev分支进行开发
# 开发代码之后，我们有两个选择
# 第一个：如果功能开发完成了，可以合并主分支
git checkout master # 切换到主分支
git merge dev # 把dev分支的更改和master合并
git push # 提交主分支代码远程
git checkout dev # 切换到dev远程分支
git push # 提交dev分支到远程
# 第二个：如果功能没有完成，可以直接推送
git push # 提交到dev远程分支
# 注意：在分支切换之前最好先commit全部的改变，除非你真的知道自己在做什么
```

### 删除分支

```shell
git push origin :dev # 删除远程dev分支，危险命令哦
# 下面两条是删除本地分支
git checkout master # 切换到master分支
git branch -d dev # 删除本地dev分支
```