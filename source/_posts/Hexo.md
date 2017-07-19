---
title: Hexo + Github pages
date: 2017-05-27 08:30:18
tags: [Hexo,技术之路]
---
## 前头
&emsp;&emsp;技术在广度发展的路上提升，为了腾飞，沉淀自己很重要。空下来的时候当然的写下了。之前都是以笔记的方式（**Evernote** 还是很强大），现在想还是写一些博文。现在市面上博文系统也是泛滥，但是还是没自己喜欢的，定制能力太差，当然对于码农怎么也得自己来搞一套啊。首先想到的当然是自己码一套代码，买个服务器搞起来，但是这样自己首先的花钱，然后还得维护系统，不好玩。然后就找到了[Jekyll][2]和[Hexo][1]，[Jekyll][2]主题没有[Hexo][1]丰富，而且是**Ruby**写的，所以选择了[NodeJs][3]的[Hexo][1]，然后结合*github pages* 来部署

## 正事
首先要安装[NodeJs][3],这里就不说了。
#### 安装[Hexo][1]
```
$ npm install hexo-cli -g
```
#### 生成Hexo博客系统
```
$ hexo init blog
```
之后会生成整个项目的目录结构，而且会安装packgejson依赖包，如果没有安装则手动安装即可
```
$ npm install
```

#### 第一篇博客
```
$ hexo new post helloworld
```
这样会在_source/_posts/ 目录下生成helloworld.md文件

#### 本地部署
```
$ hexo server
```
你将会看到 
> INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.

好了，可以开始写博客了。但是我们每个人独有自己的审美观，我们当然想选择自己喜欢的主题，那么很高兴的告诉你[Hexo][1]主题泛滥了☺, 快去下载自己个性[主题](https://hexo.io/themes/)吧。

#### 主题怎么弄呢
此处分享一篇文章，参照[NexT主题设置](http://theme-next.iissnan.com/getting-started.html)

***
至此讲道理，我们应该讲完了。但是现在讲究共享经济，好的东西要分享。你的博客当然也希望分享出去,希望别人看到。那么*Github pages*就派上用场了

#### [Github pages](https://pages.github.com) 发布博客系统

github pages怎么结合hexo呢？
##### Create a new repository
github的使用就不详说了，在github创建一个仓库,名称格式 
```
<*github-username*>.github.io
```
这样github会自动识别这个仓库为 github pages

##### 配置hexo depoy信息
在找到博客系统的配置文件 _config.yml 配置deploy参数，如下
```
deploy:
   type: git
   repo: https://github.com/gb-6k-house/gb-6k-house.github.io.git
   branch: master

```
repo 是githbub的仓库地址, branch是所选分支
##### 生成博客
生成博客之前，先清除之前生成的博客内容
```
$ hexo clean
$ hexo generate
```
命令完成之后会生成public目录，目录执行就是博客的html文件了。
##### 发布博客
```
$ hexo deploy
```
如果未设置github 的ssh信息，那么deploy的时候需要输入github账号和密码。deploy的原理实际上就是把generate生成在public目录下的文件推送到你的github repository

## 再说一句
事情都得有头有尾，总结是有必要的。如果你是个码农[Hexo][1]对你来说so easy。对你来说发布一篇博客可能经常要干的事情就是
```
$ hexo clean
$ hexo generate
$ hexo deploy
```

[1]:https://hexo.io
[2]:http://jekyll.com.cn
[3]:https://nodejs.org/en/