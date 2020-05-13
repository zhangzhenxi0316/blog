---
title: symbol set map
date: 2020-03-27 09:42:13
tags:
    -js
    -学习笔记
categories: 前端 
---

## [Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

每个symbol都不一样

### 声明symbol

```(javascript)
let hd = Symbol("描述")
//不能当作对象不能添加属性，当作特殊字符串不会重复的字符串,永远不会重复
console.log(hd) //Symbol(描述)
console.log(hd.toString())  //Symbol(描述)
console.log(hd.decrtiption)  //描述
let a = Symbol.for("描述")//第二种建立symbol的方法，但是如果发现内存中有重复的就引用过来
console.log(a.keyFor())  //查看描述只能查看Symbol。for()建立的形式
```

```(javascript)
let symbol = Symbol("不重复的")
let object = {
    name:"zzx",
    [symbol]:"baidu"
}
//普通for in 和for of遍对象遍历不到symbol只能遍历Object.getOwnPropertySymbol(obj)
//遍历Reflect.ownKeys(obj)能遍历出所有的属性包括symbol
//在对象当中可以当作私有属性来使用
```

## set集合类型

set是以中括号{}的一个数据类型
不能放重复的值，类型严格约束
语法
`let set = new Set([1,2,3]);  //参数如果是字符串那么建立的set会把字符串拆分`  
.add()  //追加值
.size  // 长度
.has(value)//检测是否有值返回bool类型
.delete(value) //删除
.clear() //清空
.values()  //打印内容
.keys()  //遍历
.entries()//遍历
.forEach(function(value,key,set){})//key==value
可以forin和for of遍历

### 类型转换

转为数组
Array.from()
[...set]
数组转set
new Set(arr)  //自动去重

### set并交差

```(js)
let a = new Set([1,2,3,4,5,6,7])
let b = new Set([2,3,4,5,7])
new Set([..a,..b])  //并集
new Set([..a].filter(item=>{return !b.has(item)})) //差
new Set([..a].filter(item=>{return !b.has(item)}))  //交
```

## WeakSet

类似set类型但是必须是引用类型,同样不能重复
`let set = new WeakSet();`
方法和set一样
弱引用  //如果其中的对象的所用引用都没了，weakset依然默认还在但是遍历不出来显示不出来

## Map

Map什么都可以作为键名的类型，类似对象 可以for of遍历 不能for in
new Map([["key",""value],["key","value"]])
.set("键名","值")  //添加
.get(key)  //取值
.delete(key)  //删除 返回布尔类型
.clear()  //清空
.has(key)  //检测
.keys()  //键
.values()  //值
entries()  //键值对
.forEach()  //循环遍历

### 类型转换

[...map]  //map转换成数组  等同于[...map.entries()]等同于for of
[...map.keys()]  //专门转换键
[..map.values()]  //专门转换值

## WeakMap

语法:
`let weakMap = new WeakMap()`
key只能添加对象，弱引用
没有keys()的方法不能for of 没有values()没有.size

## 对象类型

对象可以直接for in遍历但是不能for of

```(javascript)
let obj1={
    name:"zzx"
}

let obj ={
    name:"zzx",  //对象的属性名是字符串1和"1"是一样的
    age:18,  //不能以对象作为属性名，如果以对象作为属性名[obj]那么系统会自动把对象变为字符串
    [obj1]:"hhh"

}
console.log(obj.[obj1.string()])
console.log(obj.["[object Object]"]) //上下两种都用来打印
```


