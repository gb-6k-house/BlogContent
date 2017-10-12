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
--更新用户本地登录的密码
SET PASSWORD FOR 'username'@'localhost' = PASSWORD('password');
```

给用户在其他机器操作授权
```
grant select,insert,update,delete,create, create routine, alter routine, execute on databasename.* to 'username'@'%' identified by "password";--用户授权数据库*代表整个数据库
```
给用户在本机操作授权
```
grant select,insert,update,delete,create,alter, drop, create routine, alter routine, execute on databasename.* to 'username'@'localhost' identified by "password";
```


### PHP+nginx环境搭建安装
#### PHP安装
```
$ yum install php
```
如下命令查看php是否安装成功
```
$ php -v
```
安装php扩展 
```
$ yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring
```
PHP配置文件路径在/etc/php.ini

安装php-fpm，php-fpm通过fastCGI协议来与nginx进行通信
```
$ yum install php-fpm
```
php-fpm 配置文件路径在/etc/php-fpm.ini ,/etc/php-fpm.d/www.conf


启动 php-fpm 
```
$ service php-fpm start
```
php-fpm 确实监听9000端口，可以通过如下命令查看
```
$ netstat -lntp
```

#### 配置nginx 
参照如下
```
   server {
       listen 443;
       listen 80;
       server_name *.zzrdad.cn
       index index.php index.html index.htm;
       #php项目根目录
       root   /root/wwww/MetInfo; 
       location / {
          #     fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         try_files $uri $uri/ =404;
      }

        error_page 404 /404.html;
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
                root /usr/share/nginx/html;
         }

       location ~ \.php$ {
        try_files $uri = 404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
     }
   }
```
#### 问题
1、范围php资源时 nginx提示错误
 primary script unknown phpfpm
  解决办法
  问题出在nginx配置，root目录可能配置错误 或者nginx配置中user参数 和 php-fpm 用户user存在范围权限问题。
  ngixn缺省user=ngixn ，php-fpm中缺省user=apache，这两个用户必须对PHP项目资源有执行权限, 可以将两个user配置成一个




[1]:https://hexo.io 