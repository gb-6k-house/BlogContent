---
title: 熟练运用Visual Studio Code
date: 2018-06-08 10:57:43
tags: [技术之路, IDE]
---
### 在[VSCode][1]创建自己的代码片段

代码片段主要是一些模板，比如循环语句模板， 条件判断语句的模板。 通过代码片段让我们编写代码更快，更简单,且更容易模板化。
在vsc中使用模板也很简单， 只需要输入预先定义好的模板名前缀就能自动加入模板代码。比如使用ajax模板 请看动图(若无法查看，请点击图片加载完即可。下同)

![](/uploads/ajax-snippet.gif)

### 从VSCode市场下载代码片段模板
从市场中下载的相关语言的插件就包含大量的代码片段，因此我们可以直接下载对应的插件，重启IED就可以使用相关语言的模板了

### 创建自己的代码片段

#### 创建
我们通过创建[wpy][2]文件模板为例，来说明如何创建模板。请看动图

![](/uploads/vsc--snipped01.gif)

#### 说明
1. 创建代码片段时，需要选择在哪种语言下使用我们定义的模板。例子中我们选择vue，因为wpy我们认为是vue的文件。
2. 代码片段定义的格式如下:
```
	 "Print to console": {
	 	"prefix": "log",
		"body": [
	 		"console.log('$1');",
	 		"$2"
	 	],
	 	"description": "Log output to console"
	 }
```
其中
    "Print to console":代码片段在定义文件的名称；
    "prefix": 代码片段的前缀，使用时输入前缀就可以插入代码片段。
    "body": 我们真正定义的代码片段内容
    "description": 代码片段说明
    $0,$1,$2: 这种方式让我们在使用时，可以通过tab键快速定位模板代码的需要修改位置。比如示例中${0: ClasName}, 通过这个定义，插入代码片段时，光标自动定位到ClasName这个位置。

#### 使用创建的代码片段

请参照如下动图 
![](/uploads/vsc--snipped02.gif)


#### 示例代码
```javascript
"Wepy文件模板": {
		"prefix": "wepy",
		"body": [
			"/******************************************************************************",
			"** auth: liukai",
			"** date: 2018/7",
			"** ver : 1.0.0",
			"** desc: 说明",
			"** Copyright © 2018年 尧尚信息科技(wwww.yourshares.cn). All rights reserved",
			"******************************************************************************/\n",
			"/*样式定义*/",
			"<style lang=\"less\">\n",
			"</style>\n",
			"/*定义页面模板 */",
			"<template>\n",
			"<view>wepy model view</view>",
			"</template>\n",
			"/*页面逻辑处理部分 */",
			"<script>\n",
			"import wepy from 'wepy'",
			"export default class ${0: ClasName} extends wepy.page {\n\n}\n",
			"</script>\n"
		],
		"description": " wepy模板文件定义"
	},
```

### 高级使用
  关于代码片段的高级使用查阅[VSCode官方文档][3]

[1]:https://code.visualstudio.com/docs/editor/userdefinedsnippets
[2]:https://tencent.github.io/wepy/
[3]:https://code.visualstudio.com/docs/editor/userdefinedsnippets