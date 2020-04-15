---
title: js笔记3
date: 2020-03-25 12:50:48
tags:
    - 学习笔记
    - js
categories: 前端
---

# 【javascript】 基本数据类型

## 三元表达式

```(javascript)
let div = document.querySelector("div")
div.style.width = "200px" ? "200px" : "500px"  div宽度等于200px则不变不等于则变为500px
```

## 标签

```(javascript)
label:for(let i = 0;i <=10;i++){
    for(let u = 0 ; u<=10;u++){
        break label  //这里的break就跳出自身循环了而是跳出指定标签的循环,continue同样
    }
}
```

## for in与for of

for in和for of用来批量处理数组或对象,还可以遍历字符串
for(let key in arr) //在数组中key是索引号，在对象中是键名
for(let value in arr)//value是值ES6
for of 还可以遍历伪数组比如dom中

```(javascript)
let lis = document.querySelectorAll("li")
for (const li of lis){
    li.addEeventListener("click",function(){
        li.style.backgroundColor = "red";
    });
}
```

## typeof

typeof 关键字可以检查数据类型
用typeof检测数组返回object类型

## instanceof

用instanceof检测

```(javascript)
let hd = [];
function User(){}
let xj = new User();
console.log(xj instanceof User) //true ,Object也是true
console.log(hd intanceof Array); //true
```
<!-- more -->

## 字符串

let str = new String("baidu.com")
字符串连接:+
或者用

```(javascript)
let site = "houdunren"
let str = "com"
console.log(`${site}是${str}`)  \\内部换行也会生效这是新版的写法推荐使用${}内部可以调用函数、表达式
```

### 字符串方法

#### 格式转换

.toUpperCase()字符串转大写
.toLowerCase()字符串转小写
.trim()去除字符串前后的空白

```(javascript)
let input = document.querySelector("[name='password']")
input.addEventListener("keyup",function(){
    this.value = this.value.trim()   //使input输入框中前后不能打空格
})
```

#### 截取函数

.slice(start,end)从哪开始截取,到哪里(索引控制)，负的参数是从后数多少开始

```(javascript)
let str = `baidu.com`
console.log(str.slice(-3))  //com
```

手机号码模糊处理用slice

```(javascript)
function phone(mobile,len = 3){
    return String(mobile).slice(0,len * -1) + "*".repeat(len >= mobile.toString().length ? mobile.toString().length:len)
}
console.log(phone(13998457263,4))  //1399845****
```

.substring(strat,end)同上但是不接受负的参数
.substr(start,length)从哪里开始，截取长度，接受负数

#### 检索字符串函数

.indexOf(value,start)  //value是想搜索的值，start从哪里开始查找，返回-1是没找到
.lastIndexOf(value,start)  //从后面开始检索 从start位置向前查找
.includes(value,start)  //如果从字符串中找到value那么就返回true
.startsWith(value)  //是否以指定字符串开始
.endsWith(value)   //是否以指定字符串结束
.replace(a,b)   //用b替代a

#### 类型转换

String转为number:1.隐式转换参与除了加号的运算 2.Number(String) 3.parseInt("99ajdfjalj")结果为99，但是以字符串开头转换不了
Number转为String:1.参与字符串拼接 2.String(Number)
String转为Array: .split("分割界限")，以分割界限分割空字符串则挨个拆分
Array转换为String:.jion("分隔符")
.toString()//多用于对象调用，(用new创建的Number等)

## 标签模板

```(javascript)
let web = 'baidu'
let str = fun`www.${web}.com` //fun是标签模板
function fun(strings,...vars){  //string接收字符串,...vars接收变量  
    console.log(strings) //数组长度为2,strings[0]= "www." strings[1] = ".com"
    console.log(vars) //接收变量
}
```

模板函数接收的字符串长度是要长与变量的，如果长度不够用空字符串来代替

```(javascript)
let web = 'baidu'
let str = fun`www.${web}`
function fun(strings,...vars){
    console.log(strings)  //["www." , ""]
    console.log(vars)   // ["baidu"]
}
```


## Boolean

通过new创建的bool对象用.valueOf()来取值
Boolean()可以转换其他类型为bool类型
其他类型和bool类型比较时都转换为数字型比较

### 类型转换

!用来取反，会将数据类型先转换成bool类型之后取反
!!就能转换为boolean类型
Boolean()

## Number

### 方法

Number.inInteger(number) //检测是否为整形，返回bool类型
.toFixed(3)//保留三位小数
Number.isNaN()  //判断是否是NaN  或者用Object.is(biaodashi,NaN)
通过添加修改valueOf()可以改变数据类型转换效果
```
console.log(Number({valueOf:function(){return 999}}))  //999
```
