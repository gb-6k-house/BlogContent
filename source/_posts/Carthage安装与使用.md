---
title: Carthage安装与使用
date: 2017-06-27 09:34:27
tags: [技术之路, iOS]
---
### [Carthage][1]安装方法
#### 下载[安装包][2] 
不做过多介绍
#### Homebrew安装
1、首先确保安装了 brew，如果没有安装可以通过如下命令进行安装
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
2、安装[Carthage][1]
```
$ brew update
$ brew install carthage
```

### [Carthage][1]使用
1、进入项目所在的文件夹，然后创建文件
```
$ touch Cartfile
```
2、 添加需要第三方依赖，比如添加Reaml支持, 则在Cartfile文件添加如下代码
```
github "realm/realm-cocoa"
```
3、安装依赖
```
$ carthage update --platform iOS 
```
[1]:https://github.com/Carthage/Carthage
[2]:https://github.com/Carthage/Carthage/releases