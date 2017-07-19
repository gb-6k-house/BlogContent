---
title: 七牛JavaScript SDK 入坑
date: 2017-05-28 20:50:02
tags:  [技术之路, JavaScript]
---
[七牛官方SDK](https://developer.qiniu.com/kodo/sdk/1283/javascript)，上传文件使用了插件Plupload，如果通过npm方式安装 Plupload之后，就会存在问题，七牛sdk要求的Plupload版本是 2.1.1 ~ 2.1.9，而这个版本Plupload已经不提供下载了。Plupload之后的版本mOxie对象已经删除了。所以官方SDK根本用不了

#### 解决办法
```
const moxie = require('plupload/js/moxie.min.js');
if (!global.mOxie) {
  global.mOxie = {
    Env: moxie.core.utils.Env,
    XMLHttpRequest: moxie.xhr.XMLHttpRequest
  };
};
global.plupload = require('plupload/js/plupload.min.js');
require('qiniu-js/dist/qiniu.js');
```