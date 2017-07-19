---
title: centOS 7 开发环境搭建
date: 2017-05-28 20:03:29
tags: [技术之路,后台, Nodejs, Mysql]
---

&emsp;&emsp; 这是一篇讲解如何在CentOS 7 系统上搭建NodeJs 开发环境，主要包括


### [Nodejs][1]安装

#### **方式一 源码安装**


1. 更新编译源码的开发工具 
```
$ yum -y groupinstall "Development Tools"
```
2. 安装Node.js，首先进入/usr/src文件夹，这个文件夹通常用来存放源代码 
```
$ cd /usr/src
```
3. 从node官网获取最新压缩的源代码。我们以安装7.6.0.为例：
```
$ wget https://nodejs.org/dist/v7.6.0/node-v7.6.0.tar.gz
```

4. 解压源文件
```
$ tar zxf node-v7.6.0.tar.gz
```
5. 执行配置脚本进行预编译处理
```
$ cd node-v7.6.0
$ ./configure
```
6. 开始编译 
```
$ make
```
7. 编译完之后安装 , 默认情况下Node安装在/usr/local/bin目录下
```
$ make install
```
8. 至此node安装完成 node –v可以检测是否安装成功。

#### **方式二 yum安装**
1. 获取yum资源
```
$ curl --silent --location https://rpm.nodesource.com/setup_7.x | bash -
```
2. 安装
```
$ yum install -y nodejs
```
淘宝镜像
> npm install -g cnpm --registry=https://registry.npm.taobao.org

### [PM2](http://pm2.keymetrics.io)进程管理
![](/uploads/pm2Logo.png)

安装：npm install -g pm2

详细使用参照[文档](http://pm2.keymetrics.io/docs/usage/cluster-mode/)

### 安装Mysql
检查是否已经安装
```
$ rpm -qa|grep mysql
```
如果已经安装，则可以通过命令删除
```
$ rpm –e mysql
```
查找yum可以安装的msql版本
```
$ yum list|grep mysql
```
如果没有repo源，则可以先现在rpm源
```
$ cd /usr/src
$ wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```
安装rpm包
```
$ sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
```
安装mysql
```
$ yum install mysql-server
```
至此安装已经完成

1、启动　　

MySQL安装完成后启动文件mysql在/etc/init.d目录下，在需要启动时运行下面命令即可。　　
```
$ /etc/init.d/mysql start　
```
或者
```
service mysql start
```
2、停止　
```
service mysql stop　
```

基本sql
创建数据库
```
CREATE DATABASE `databasename` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```
创建用户
```
create user 'username'@'%' identified by ‘password’;
```
给用户在其他机器操作授权
```
grant select,insert,update,delete,create on databasename.* to 'username'@'%' identified by "‘password’";--用户授权数据库*代表整个数据库
```
给用户在本机操作授权
```
grant select,insert,update,delete,create,alter, drop on databasename.* to 'username'@'localhost' identified by "‘password’";
```
[1]:https://hexo.io 