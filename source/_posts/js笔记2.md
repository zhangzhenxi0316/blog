---
title: js基础知识
date: 2020-03-24 19:33:04
tags:
    - js

categories: 前端
---


## var let const

const定义常量值不能改变(常量所引用的地址不能变，引用的是个对象则能改变)
let是后出的声明修饰推荐使用

### 变量提升

用var声明变量有变量提升
用let和const没有变量提升必须先声明后使用

```(javascript)
let web = "str";
function fun(){

    let web = "str1";  //在这个代码块里必须先声明才能使用
    console.log(web);

}
fun(); //会报错
```

### 不同声明作用域不同及全局污染概念

var let const 都有函数作用域的概念这点是共有特点
但是var没有块作用域

```(js)
{
    var web = "baidu.com";
    let i = 9;
}
console.log(web)  //baidu.com  var没有块作用域
console.log(i)  //报错，let有块作用域
```

声明变量的时候一定要加声明，要不会有全局污染，可以用严格模式来检查时候有忘记声明的错误，会报错提醒我们
只需要在代码中引入
`"use strict";`
**推荐使用let和const声明变量**

```(javascript)
var i = 99;
for(var i = 0; i<5; i++){  //若用let声明变量i那么i的作用域只在for这个块作用域中

    console.log(i);

}
console.log(i)  //for用var声明的变量i没用块作用域污染到了全局此时i为5
```

若是我们自己写个js插件，那么用var声明的变量可能会影响到后续插件或者开发，所以需要用到执行函数比如

```(javascript)
(function(){
    var $ = window.$ = {};
    $.web = 'baidu.com';
    $.getUrl = function(){
        return url;
    }
}.blind(window)())
```

用let声明更简单
let有块作用域

```(javascript)
{
    var $ = window.$ = {};
    $.web = 'baidu.com';
    $.getUrl = function(){
        return url;
    }
}
```

在后续开发就用给$封装的函数获取里面需要的变量就好了，不会污染到全局
<!-- more -->

### var和let的区别

用var声明的变量会改变window的属性let不会
同一作用域中var重复声明同名变量不报错，保存最后一次的声明，let和const不允许重复声明

### Object.freeze冻结变量

如果用const定义的常量是引用数据类型，如对象那么这个常量对象内部值是可以改变的

```(js)
const HOST = {
    url:"baidu.com"
    port:443
}
//用Object.freeze(HOST)静态方法可以冻结常量此时下列不能修改
HOST.port = 888
console.log(HOST.port)  //888没用冻结可以修改引用数据类型常量中的属性

```

## 传值与传址

内存比较小传值

```(js)
let a = 1;
let b = a;
```

内存大传址如

```(js)
let a = {}
let b = a
```

## null与undefined 

空的基本数据类型是undefined
空的引用类型是null

## 不知道怎么分类...

字符串拼接可以用${}
如

```(js)
var name = zzx;
‘[name=${name}]’   //相当于[name=zzx]
```
