---
title: npm 发布代码
date: 2017-08-25 11:31:14
tags:  [技术之路, JavaScript, 开源]

---
### package.json关于发布的配置

"files" 包含需要发布的文件夹或文件
"main" 指定外部程序require该模块时的文件


### 发布
#### 登录npm
```
$ npm login 
```
#### 发布代码
```
$ npm publish
```
发布的时候如果发布新包，可能收到下面提现，需要邮件进行验证

 npm ERR! you must verify your email before publishing a new package: https://www.npmjs.com/email-edit : node-yourshares


