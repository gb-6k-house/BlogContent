---
title: 快速搭建Typesctipt开发环境
date: 2018-06-14 09:42:45
tags:
---
手动添加TypeScript是费时费力的，但是好在有[yeoman.io][1]提供的yo工具，我们可以基于yo开发很多工具支持相关工作流的自动化。
本文将学习如何在yo下快速搭建TypeScript的开发环境的。

1. 安装yo
```
npm install -g yo
```
2. 安装TypeScript的Node开发环境生成器
```
npm install -g generator-node-typescript
```
3. 创建项目文件夹
```
mkdir my-new-project && cd $_
```
4. 初始化项目
```
yo node-typescript
```
5. 生成新的类与相应的测试文件
```
yo node-typescript:classlib MyNewClass [--mocha | --ava]
```

[1]:http://yeoman.io
