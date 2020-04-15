---
title: js笔记1
date: 2020-03-24 09:48:36
tags:

    - 学习笔记
    - js

categories: 前端
---

# 【javascript】 面向对象和类的继承

## 类创建

语法：

```(javaScript)
class Name{

    //class body
    constructor(uname,age){  //构造函数
        this.uname = uname;
        this.age = age;
    }
    sing(song){  //方法
        console.log(song)
    }

}

```

创建实例：

```(javaScript)
var zzx = new Name('彭于晏',18);
var tsy = new Name('刘德华',19)
console.log(zzx.uname)//彭于晏
console.log(tsy.age)//19
tsy.sing('冰雨')
```

**类必须使用new实例化对象**
constructor()方法是类的构造函数(默认方法) , 用于传递参数，返回实例对象，通过new命令生成的实例时，自动调用该方法。
**规范**
类名首字母大写，类名后不要加小括号，构造函数不要加function，类中函数都不用加function，方法之间不需要，分隔开
<!-- more -->

## 类的继承

Son继承Father

```(javaScript)
父类
class Father{

    constructor(){
        //构造函数
    }
    money(){
        //方法
    }

}
子类
class Son extends Father{

}

```

子类继承了父类的属性和方法

```(javaScript)
var son = new Son();
son.money();
```

**super关键字**
super必须在子类的this之前调用
super 关键字用于调用对象的父对象上的函数。
super()//调用父类构造函数
super.money()//调用父类方法

**子类可以继承方法并扩展自己的方法**
注意若是继承的父类方法，方法里面的this指向的是父类，需要调用super()父类的构造函数
**例子**

```(javaScript)
class Father{

    constructot(x,y){
        this.x = x;
        this.y = y;
    }
    sum(){
        console.log(this.x - this.y)
    }

}
class Son extends Father{

    constructor(x,y){
        super(x,y);
        this.x = x;
        this.y = y;
    }
    subtract(){
        console.log(this.x - this.y);
    }

}

var son = new Son(5, 2);
son.sum();
son.subtract();
```

调用父类的sum方法，sum方法中this指向的是父类实例化出来的对象，所以需要在子类构造函数中调用父类的构造函数将参数传向父类使得sum方法找到this.x和this.y *super()需要在子类this之前*
---
ES6中类没有变量提升，所以需要先定义类，才能类实例化对象
类中共有属性和方法一定要加this使用
注意this指向，那个对象调用就指向哪个对象
---
```(javaScript)
class Father{

    constructot(x,y){
        this.x = x;
        this.y = y;
        this.btn = document.querySelector('button')
        this.btn.onclick = this.sing;
    }
    sing(){
        console.log(this.x)
    }

}
```
**注意btn点击后sing中的this指向了btn按钮对象，若想输出实例化对象得x需要在constructor中用其他变量复制一下this**
