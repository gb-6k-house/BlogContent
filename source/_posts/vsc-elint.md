---
title: VSCode中根据ESlint自动格式化代码
date: 2018-06-08 10:57:43
tags: [技术之路, IDE, VSCode]
---
### 为什么？
我们在编写javacript代码时，往往会用ESLint来进行代码格式检测，避免运行期的一些错误出现。
那么我们如何在写代码的同时，IDE就能自动按eslint配置检测代码呢，并且通过IDE快捷格式化时,能自动按eslint要求格式化代码呢
### 如何开始
VSCode强大的功能就是插件，那么我们想到的自然是通过插件来实现。搜索eslint，安装插件。
![](/uploads/vsc-eslint-01.png)

### 如何配置
在VSCode中，点击 文件(或Code)>首选项>设置，进入设置页面。找到ESLint的设置。
然后配置我们需要进行ESLint检查的文件类型，缺省是支持javascript。如果我们需要对Vue文件进行检查，可以按如下配置
```
    "eslint.validate": [
    "javascript",
    "javascriptreact",
    "javascriptreact",
    {
        "language": "vue",
        "autoFix": true
    }
],
```
我们在eslint.validate中添加对vue语言的检查。保存之后所有vue文件中未满足eslint格式要求的都会自动提示。
### 如何自动格式化
通过上面的步骤，只是在编写文件时提示了我们那些格式是不符合规则要求的，那么如何对文件按要求格式化呢。有如下两种方式。
#### 通过[prettier][1]
VSCode本身就支持prettier的设置。 同理打开设置文件，找到prettier，配置如下参数
```
"prettier.eslintIntegration": true,
```
保存文件。然后在代码编辑器中通过VSCode快捷键格式化代码时，会自动按eslint要求进行格式化。
#### 通过ESlint插件
通过设置eslint插件的参数autoFixOnSave，使文件在保存时自动按eslint要求进行格式化。
```javascript
// Turns auto fix on save on or off.
"eslint.autoFixOnSave": true,
```

[1]:https://prettier.io/docs/en/index.html

