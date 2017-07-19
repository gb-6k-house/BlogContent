---
title: express+formidable 搭建文件服务器
date: 2017-06-12 16:42:56
tags: [技术之路,express, 文件服务器]
---

#### 文件下载
文件下载，可以利用 Express内置的 express.static 托管静态文件，例如图片、CSS、JavaScript 文件等
```
app.use(express.static('public'));
```
这样就可以访问public目录下的资源了
```
http://localhost:3000/images/bg.png
```
如果资源文件放在多个目录，则可以多次调用 express.static 中间件
```
app.use(express.static('public'));
app.use(express.static('files'));
```
访问静态资源文件时，express.static 中间件会根据目录添加的顺序查找所需的文件。

如果你希望所有通过 express.static 访问的文件都存放在一个“虚拟（virtual）”目录（即目录根本不存在）下面，可以通过为静态资源目录指定一个挂载路径的方式来实现, 如下所示
```
app.use('/static', express.static('public'));
```
现在，可以通过带有 “/static” 前缀的地址来访问 public 目录下面的文件了。

```
http://localhost:3000/static/images/bg.png
```
#### 文件上传
文件上传主要是使用了第三方库[formidable](https://github.com/felixge/node-formidable), 主要是用来解析表单数据.
安装
```
$ npm install --save formidable
```
参数说明 
 form.encoding = 'utf-8' 设置表单域的编码
 form.uploadDir = "/my/dir"; 设置上传文件存放的文件夹，默认为系统的临时文件夹，可以使用fs.rename()来改变上传文件的存放位置和文件名
 form.keepExtensions = false; 设置该属性为true可以使得上传的文件保持原来的文件的扩展名。
 form.type 只读，根据请求的类型，取值'multipart' or 'urlencoded'
 form.maxFieldsSize = 2 * 1024 * 1024; 限制所有存储表单字段域的大小（除去file字段），如果超出，则会触发error事件，默认为2M
 form.maxFields = 1000 设置可以转换多少查询字符串，默认为1000
 form.hash = false; 设置上传文件的检验码，可以有两个取值'sha1' or 'md5'.
 form.multiples = false; 开启该功能，当调用form.parse()方法时，回调函数的files参数将会是一个file数组，数组每一个成员是一个File对象，此功能需要 html5中multiple特性支持
 form.bytesReceived 返回服务器已经接收到当前表单数据多少字节
 form.bytesExpected 返回将要接收到当前表单所有数据的大小

**formidable.File**对象

file.size = 0 上传文件的大小，如果文件正在上传，表示已上传部分的大小
file.path = null 上传文件的路径。如果不想让formidable产生一个临时文件夹，可以在fileBegain事件中修改路径
file.name = null 上传文件的名字
file.type = null 上传文件的mime类型
file.lastModifiedDate = null 时间对象，上传文件最近一次被修改的时间
file.hash = null 返回文件的hash值
 可以使用JSON.stringify(file.toJSON())来格式化输出文件的信息


**结合expresss路由使用**
```
router.post('/upload', function (req, res) {
    const form = new formidable.IncomingForm();
    form.uploadDir = config.imageDir;//上传文件的保存路径
    form.keepExtensions = true;//保存扩展名
    form.maxFieldsSize = config.maxImageSize;//上传文件的最大大小
    form.parse(req, (err, fields, files) => {
        //...
    })
})
```
**修改文件名**
如果想修改文件名，可以在form.parse回调中修改
```
form.parse(req, (err, fields, files) => {
        if (err) {
            res.status(err.status || 500);
            return
        }
        console.log(files.file.path);
        console.log('文件名:' + files.file.name);
        var t = (new Date()).getTime();
        //生成随机数
        var ran = parseInt(Math.random() * 8999 + 10000);
        //拿到扩展名
        var extname = path.extname(files.file.name);

        //path.normalize('./path//upload/data/../file/./123.jpg'); 规范格式文件名
        var oldpath = path.normalize(files.file.path);

        //新的路径
        let newfilename = t + ran + extname;
        var newpath = './public/images/' + newfilename;
        console.warn('oldpath:' + oldpath + ' newpath:' + newpath);

        fs.rename(oldpath, newpath, function (err) {
            if (err) {
                console.error("改名失败" + err);
                res.status(500);
            }
            res.status(200);
        });
    })
```