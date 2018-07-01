---
title: 关于javascript的Object
date: 2018-06-15 10:57:28
tags: [技术之路, JavaScript]
---

javascript的Object对象提供了操作对象的方法。下面对几个重要的方法进行讲解

#### getPrototypeOf
getPrototypeOf 方法获取对象的原型链prototype

#### getOwnPropertyNames和keys

- getOwnPropertyNames获取属于自己的可枚举或不可枚举的属性，但不包括原型链上的属性性。
- kyes获取属于自己的可枚举属性，但不包括原型链上的属性。

下面通过例子说明
```
function Test(){
    this.title = 'abc'
}
Test.prototype.a = function (params) {
}
Test['b'] = function () {}
var t =  new Test()
t['c'] = function () {}

console.log(Object.getOwnPropertyNames(Test)); //[ 'length', 'name', 'prototype', 'b' ]
console.log(Object.getOwnPropertyNames(t));// [ 'title', 'c' ]
console.log(Object.keys(t));// [ 'title', 'c' ]
console.log(Object.getOwnPropertyNames(Object.getPrototypeOf(t)));//[ 'constructor', 'a' ]


```
