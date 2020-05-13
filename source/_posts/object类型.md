---
title: object类型
date: 2020-05-10 15:39:02
tags:
---
## Object类型
object类型的每个实例都有下列属性和方法
constructor 保存着用于创建该对象的构造函数
.hasOwnProperty(propertyName) 检查给定的属性是否在当前对象的实例中而不是在其原型中
.isPrototypeOf(obj) 用于检查传入的对象是否是当前对象的原型
.propertyIsEnumerable(propertyName) 用于检查当前对象中的该属性是否可以使用for in去枚举
.toLocaleString() 返回对象的字符串表示，该字符串与执行环境的地区对应
.toString() 放回对象的字符串表示
.valueOf() 返回对象的字符串。数值，布尔值表示，
