---
title: WePy入坑过程
date: 2018-06-05 11:00:05
tags:  [技术之路, 小程序]
---
### 坑1

按教程新建[WePy][1]工程时，在微信开发中工具中编译会报如下错误
![](/uploads/WePy1.png)
此时点项目详情，按如下设置项目
![](/uploads/WePy2.png)
WePy项目编译通过

### Unexpected token u in JSON at position 0
这个坑不知道真的是害死人， 本部会找不到问题所在在app.wpy文件中定义页面索引时，pages中的页面不能以‘/’开头，例如
```
pages: [
      '/pages/index'
    ]
```
这样定义编译可以通过，但是在微信开发这工具中运行会报错
```javascript
 /pages/index.json 文件解析错误  SyntaxError: Unexpected token u in JSON at position 0
```
正确应该这样设置页面路径
```
pages: [
      'pages/index'
    ]
```
### 关于使用$redirect或$navigate等,在页面之间转递数据问题
1、直接将数据做个$navigate等的参数进行转递，无法转递Object。
```
// page1.js
  this.$navigate('./page2',{param1:{}});

  // page2.js
  onLoad (params, data) {
     //这里param1传递过来之后变成了字符串"[object Object]", 实际上应该是一个Object 这是底层框架的BUG
      console.log(data.params.param1); //"[object Object]"
  }
```
2、通过$preload传递Object
```
  // page1.js
  this.$preload('param1', {});
  this.$redirect('./page2');

  // page2.js
  onLoad (params, data) {
      console.log(data.preload.param1); // 可以正确拿到对象
  }
```

[1]:https://tencent.github.io/wepy/