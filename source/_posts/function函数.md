---
title: function函数
date: 2020-05-10 23:59:39
tags:
---
函数function
每个函数都有个arguments的属性，是个数组，里面存的是传进函数的参数，长度由传进去多少参数而定 ，
函数定义时的形参个数可以和传进去的实际参数不一样，行参只是方便理解，arguments.length可以看实参的个数
```
//定义一个求和函数，传进去的参数个数不一定
function sum(){
    let result = 0;
    for( let value of arguments){
    result += value;
    }
    return result;
}
sum(1,2,3,4,5,6)
```

js的函数没有重载，重复定义会覆盖
