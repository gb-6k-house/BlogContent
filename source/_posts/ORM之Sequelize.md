---
title: ORM之Sequelize
date: 2017-06-15 10:12:09
tags: [技术之路, Nodejs]
---

### What is ORM ?
 关于ORM的介绍文章网上有很多，这里就不做过多的讲解了。ORM这些年的反正好像没什么进展，这是为什么呢？ 只有有时间在谈论讨论。

 ### [Sequelize][1]
[Sequelize][1]是Nodejs下面优秀的ORM框架，Github上[源码][2]的star数已经超过1W了。
#### Assocations
[Sequelize][1] Assocations 可以描述表与表直接的管理关系，通过设置表的Assocations可以进展多表关联查询。关于详细的Assocations使用参考官方[文档][1]。
#### belongsTo* 和 has* 的区别
假设两个模型User(用户) 和 Artical(文章) 显然 User:Artical是1:n的关系。在连接关系时可以
User.hasMany(Artical) 或者 Artical.belongsTo(User)。 这两种方式中的任意一种都会在表Artical中创建一个外键。那么区别是什么呢？
区别在：如果要通过User查询Artical,即:
```
 User.find*({include:[{model: Artical}]})
 ```
那么必须给User对象设置
```
User.hasMany(Artical)
```
如果在查询Artical的同时需要返回User数据，,即:
```
 Artical.find*({include:[{model: user}]})

```
那边必须给Artical对象设置
```
Artical.belongsTo(User)
```

[1]:http://www.nodeclass.com/api/sequelize.html
[2]:https://github.com/sequelize/sequelize