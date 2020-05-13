---
title: label语句和with语句
date: 2020-05-10 23:16:40
tags:
---
## label 语句
```
label : for(let i = 0; i < 10; i++){ }
label语句是在代码中添加标签，配合break和continue来使用跳转到指定的标签 
```
## with语句
严格模式下是不允许用with的
with(expression) statement
作用是将代码的作用域设置到一个特定的对象中
```
with(location){
//这里面的变量会被认定是局部变量找不到定义的话看location对象中是否有同名的属性
    let qs = search.substring(1);
    let hostName = hostname;
    let url = href
}
```
