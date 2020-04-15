---
title: js笔记6
date: 2020-03-29 16:00:46
tags:
    -js
    -学习笔记
categories: 前端
---
## 函数
可以一些函数封装在对象中
```(javascript)
let math = {
    sum(a,b){
        console.log(a+b);
    };
    reduce(a,b){
        console.log(a-b);
    };
};
math.sum(10,7)  //通过对象函数调用
```
在外边定义的函数默认定义在window中
用let声明变量用匿名函数赋值，一般定义在类中
函数也是引用类型
函数可以有默认参数值，默认参数一般放在后面
立即执行函数来封装函数可以不污染全局环境或者用块作用域
```(javascript)
(function(window){
    function hd(){}
    function show(){}
    window.js1 ={hd,show}
})(window)
```
arguments是函数接收的参数 伪数组，用[...arguments]转换成数组用数组方法可以遍历操作
在最新的js中可以用...args来接收参数，args就是数组不用转换
### 箭头函数
()=>{}
事件处理不方便用箭头函数
箭头函数中的this是父级的this，普通函数的this是window 方法函数的this是对象
### 递归函数
阶乘函数
```(javascript)
function factorial(num){
    if(num==1){
        return 1;
    }
    return num * factorial(num-1)   //递归操作
}
```
### 方法
对象中的函数是方法
方法中的this是房前对象的引用，对象中的方法中的this指向对象，对象中的普通函数指向window
```
let Lesson = {
    site:"后盾人"
    lists:["js","css","mysql"]
    show:function(){
        const self = this;              //指向当前对象
        return this.lists.map(function(value){
            return self.site+value;
        },this)    //map函数可以传第二个参数，会把函数中的this指向传的参数
    }
}
```
### 回调函数
事件函数就是回调函数
```
let Dom ={
    site:"houdunren"
    bild:function(){
        const button = document.querySelector("button");
        button.addEventLister("click",function(event){
            console.log(this);   //用function里面的this指向调用者按钮，用()=>中的this指向父级就是Dom
            console.log(this.site + event.target.innerHTML);//event.target指向函数出发者button
        })
        const buttons = document.querySelector("button")
        buuttons.forEach(elem=>{
            console.log(this)  //指向Dom
            elem.addEeventListener("click",()=>{
                console.log(this)    //指向Dom
            })
        })
    }
}
```
## 展开语法
等式右边是放
```
let arr = [1,2,3]
let [a,b,c] = [...arr]   //将arr释放之后结构赋值
```
等式左边就是收
```
let [a,...arr] = [1,2,3,4]  //收2，3，4
```
函数参数里的点语法就是收

## apply call bind
### call
fun.call(obj,args)  //将fun函数的this改为obj的args是传给fun的参数,并立即执行函数
```
function User(name){
    this.name = name;
}
let lisi = new User("李四")  //工厂构造函数创建对象
let obj = {url:"baidu"}
User.call(obj,"zzx")   //将User函数的this指向obj,调用函数相当于给obj添加了个属性
console.log(obj)    {url:"baidu",name:"zzx"}
```
### apply
fun.call(obj,args)  //不同于call的是args需要是数组
```
let arr = [1,2,3,4,5]
console.log(Math.max(...arr))
console.log(Math.max.apply(Math,arr))  //this不变参数是数组效果一样，
```


获取地址案例
```
function Request(){
    this.get = function(params){
        let str = Object.keys(params).map(k=>`${k}=${params[k]}`).join("&");
        let url = `https://api.houdunren.com?${this.url}/{str}`
        document.write(url + "<hr/>")
    };
}
function Atricle(){
    this.url = "article/lists"
    Request.apply(this)
}
function User(){
    this.url = "user/lists"
    Request.apply(this)
}
let user = new User()
user.get({id:1,cat:"js"})
let atricle = new Atricle()
atricle.get({id:2,role:"admin"})
```
### bind
不立即执行，和call相似
fun.bind(obj,args)()  //后面加()可以立即执行
let fun1 = fun.bind(obj,args)  //通过返回值可以得到新的函数


## 环境
```
function hd(){
    let n = 1;
    return function sum(){
        n++;
        console.log(n)
    }
}
let a = hd();  //外部引用，环境就不会删除多次调用a()n会++,但是单独调用两次hd()()n不会++
```

### 块作用域
let,const有块作用域{}
var没有块作用域
```
for(let i = 0;i<=3;i++){  //每个i都对应一个计时器  每次执行for都建立一个环境每个环境中有单独的定时器和自增后的i，i在块里
    setTimeout(function(){
        console.log(i);  //0123  
    },1000)
};
for(var i = 0;i<=3;i++){  //i是全局作用域i++到4开始执行定时器，定时器回去全局找i这时候i已经是4了
    setTimeout(function(){
        console.log(i);  //4444
    },1000)
}
```

## 闭包
子函数能访问父元素的变量就叫闭包


