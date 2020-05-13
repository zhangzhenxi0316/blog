---
title: number数值类型注意
date: 2020-05-09 22:47:36
tags:
---
## 数值运算
number数值的运算0.1 + 0.2是不等于0.3的，
因为在js采用的标准，浮点数存储空间是大于整数的，0.1存储在计算机中是二进制的形式，浮点数存储64位双精度，11符号位 11指数位 52有效位，0.1和0.2转换成二进制
是无限循环的二进制，但是存储有限所以计算机会取近似的数，所以加起来不是0.3 可以用.toFixed(n)方法来规定小树的位数

Number.MIN_VALUE  数值的最小值
Number.MAX_VALUE  数值的最大值
超过最大值的就会被认定为Infinity 小于最小值被认为是-Infinity  用isFinite(number)判断是否在最大最小之间
用Number.NEGATIVE_INFINITY 和 Number.POSITIVE_INFINITY 也可以取到负的和正的无穷

NaN 非数值，与任何数值不相等包括自身，isNaN( )来判断是否为NaN
isNaN(obj) 该方法如果检测对象会先调用valueOf()方法 判断返回值是否能转换成数值型，不能则调用对象的toString()方法判断返回值能不能转换成数值进行判断
