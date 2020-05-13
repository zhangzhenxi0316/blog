---
title:【javascript】日期与数组
date: 2020-03-26 10:59:13
tags:
    - js
categories: 前端
---


## Math

### 常用方法

Math.max(1,2,3,4,5) //求最大值  如果参数是个数组，那么用apply()方法可以应用到数组当中
Math.ceil()  //向上取整  7.1 => 8
Math.floor()  //向下取整  6.9 =>6
Math.round()  //四舍五入位最近整数
Math.random()  //返回0-1之间随机数

```(javascript)
取0-x的整数随机数
Math.floor(Math.random()*(x+1);
取a-b之间随机整数
Math.floor(Math.random()*(b-a)+a)
```

## Date

通过new创建

```(javascript)
const date = new Date()
console.log(date)  //Thu Mar 26 2020 11:27:33 GMT+0800 (中国标准时间)
const date1 = Date()  
console.log(date1)  //是字符串类型的时间Thu Mar 26 2020 11:27:33 GMT+0800 (中国标准时间)
```

传参数能转换成格式的时间

```(javascript)
new Date("1990-9-22 3：22：18")  //index.html:11 Sat Sep 22 1990 03:22:18 GMT+0800 (中国标准时间)
new Date(1990,9,22,3,22,18)
let arr = [1990,9,22,3,22,18]
new Date(...arr)   //展开语法
```

### 日期时间戳转换

**日期转时间戳**
1.日期时间*1  new Date("1990-9-22 3：22：18")*1  //656536938000
2.Number(date)
3.date.valueOf()
4.date.getTime()
**时间戳转日期**
new Date(时间戳)

### 方法

Date.now()  //时间戳，距离1970年1月1日的毫秒数

```(javascript)
let start = Date.now()
function(){}
let end = Date.now()
console.log(start-end)   //可以计算程序执行时间
```

上述方法zaiconsole中给我们有实现

```(javascript)
console.time("for")
//程序
console.timeEnd("for")
控制台输出:for: 0.010009765625ms
```

.getTime()  //获取时间戳
.getMonth()  //返回月  从0开始，+1是真实月份
.getFullyear()  //获取年份
.getDate()  //获取日
.getHours()  //获取小时
.getMinutes() //获取分钟
.getSeconds()  //获取秒数
.getDay()  //获取星期几 0-6

日期格式化函数自己封装:

```(javascript)
function dateFormat(date,format = "YYYY-MM-DD HH-mm-ss"){
    const config = {
        YYYY:date.getFullYear(),
        MM:date.getMonth()+1,
        DD:date.getDate(),
        HH:date.getHours(),
        mm:date.getMinutes(),
        ss:date.getSeconds(),
    }
    for(let key in config){
        format = format.replace(key,config[key])
    }
    return format;
}
let date = new Date("1990-9-22 3:22:18")
console.log(dateFormat(date, "YYYY年MM月DD日 HH时mm分ss秒"));
```

优秀的[日期js库](http://momentjs.cn/)

<!-- more -->

## 数组

引用类型的特性

```(javascript)
let arr1 = [1,2,3]
let arr2 = arr1;
arr2[1] = "hhh"
console.log(arr1[1])  //“hhh” 两者引用的是一个数据空间
```

console.table()打印更加清晰

通过new创建数组如果只输入一个值比如
`let arr = new Array(6)  //实际上创建了长度为6的数组为不是[6]`
通过新版的Array.of()避免了缺陷
`let arr = Array.of(6)  //[6]`

### 数组方法

#### 查找方法

.indexOf(value,start) //查找指定元素 查不到返回-1  严格类型匹配
.lastIndexOf(value,start)  //从后开始查找 start是负的
.includes(value,start)  //查找指定元素，返回布尔类型

```(javascript)
let lesson = [{name:"js"},{name:"html"},{name:"css"}]
console.log(lesson.includes({name:"js"})) //返回flase，因为这里的{name:"js"}是另一个内存空间共和数组中的不是一个
console.log(lesson.find((item) => {return item.name =='css'}))//能找到
```

.find(function(item){return 条件}) //根据条件查找，返回查找到的值，没找到返回undefined
.findIndexOf()//和find一样只不过返回索引值

```(javascript)
let arr = [1,6,45,9,7]
let result = arr.find(item => {return item ==9})
console.log(result);  //9
```

#### 数组排序

.sort(function(){return a-b;})  //如果是a-b则是-1从小到大，b-a是1从大到小，返回新数组

```(javascript)
let cart = [
    {name:"ipone",price:12000},
    {name:"imac",price:18000},
    {name:"ipad",price:3200},
]
cart = cart.sort((a,b) =>{return b.price-a.price;})  //从大到小
```

#### 字符串和数组转换

.toString() //将数组连成字符串用,连接
String(arr)  //类型转换为字符串
.join("分隔符") //指定分隔符连接
Array.from(obj,function(item){})  //可以转换带有length属性的对象比如string
通过Array.from()将节点元素转换成数组,里面的函数参数是迭代遍历数组的每一个值

```(javascript)
let div = document.querySelector("div")  伪数组有length属性
let divs = Array.from(div,function(item){item.style.backgroundColor = "red" return item})
```

#### 对数组进行操作增删改

**以下均对原数组进行修改**
.push()  //最后追加 返回值是长度
.pop()  //最后删除 返回值是长度
.unshift() //从前面压入一个值 ，返回值是长度
.shift()  //从前面移除函数
.fill(”填充元素“,start,end)  //填充数组 不包括end
.splice(start,length,args)  //删除长度指定数组，从哪开始删除多少，删除的地方添加args
.copyWithin(target,start,end)  //数组内移动，target目标位置，start从哪开始，end结束位置不包括

```(javascript)
let arr = [1,3,45,6,7]
arr.copyWithin(2,0,1)  //移动目标在2处，从0-1也就是0这一个数移动到2覆盖它
console.log(arr);  //  [1,3,1,3,7]
```

```(javascript)
splice可以实现删除追加修改的操作
let arr = [1,2,3,4,5]
arr.splice(0,1,"zzx")  //从0开始截取1个并在截取位置添加字符串zzx
arr.splice(1,0,"zzx")  //在1的位置添加zzx其他元素顺势往后
```

**不对原数组修改，返回新数组**
.slice(start,end)  //截取数组，不包含end
.join("连接符") //连接成字符串
.concat(arr)  // 连接数组 和展开语法一样效果

#### 清空数组的方法

arr =[]
arr.length = 0
arr.splice(0,arr.length)

### 数组的展开语法

```(javascript)
let arr1 = ["a","b"]
let arr2 = ["c","d"]
arr1 = [...arr1,...arr2] //合并了两个数组...代表数组的展开后面加上数组名
```

可以转dom对象伪数组为数组之后就i可以用数组方法了

```(javascript)
let div = document.querySelector("div")  伪数组有length属性
let divs = [...div]
```

展开语法还可以用来接受多个参数合成数组

```(javascript)
function sum(...args){
    return args.reduce((total,item) =>{
        return total+item
    },0)
}
sum(10,4,5)  //19
```

```(javascript)
let [...arr] = "baidu"
console.log(arr) //["b","a","i","d","u"]
```

### 数组的结构赋值

```(javascript)
let arr = ["zzx",20];
let [name,year] = arr;  //name ="zzx"  year = 20
```

```(javascript)
let [name,...args] = ["后盾人","houdunren",2010]  //name="后盾人" args = ["houdunren",2010]
```

### 数组迭代方法

使用迭代方式遍历数组
.keys()//方法用于从数组创建一个包含数组键的可迭代对象。返回键
.values() //方法用于从数组创建一个包含数组值的可迭代对象。返回值
.entries() //也是可迭代对象但是又键值对 返回键值对
.keys().next() //可以取出{value:0,done:false}//0是索引，false是代表是否迭代完成代表后面还有数据
.values().next() //通过{value,done}=values.next()能分别接收到value和done
.entries().next() //他的values是个数组，包含键和值

```(javascript)
let arr = ["baidu",".com"]
for(const value of arr.values()){
    console.log(value)  //能取到值
}
```

```(javascript)
for (const [key,value] of arr.entries()){
    console.log(value)
}
```

Array.from() //上面有介绍

.map(function(value, key, array){})//迭代方法，参数是函数，对数组每个值都进行一次函数

```(javascript)
let arr = [3, 4, 5];
let str1 = arr.map((value, key, array) => {  //value数组值，key数组索引，array数组本身
    return value + 1;      //=>js箭头函数
});
console.log(str1);
```

.every(function(item,key,array){return 条件})//判断是否都满足条件，如果有一个不满足立马返回false
.some(function(value, index, array){})  //检查某些数组值是否通过了测试。有一个满足立马返回true

```(javascript)
let arr = [33,44,55]
console.log(arr.some((value, index, array) =>{
    return value > 18;    //返回true 有条件满足
}))
```

.reduce(function(total,value,key,array){},total)  //数组中每个都执行函数,可以传默认值total，之后total就是上一次执行的返回值//不传参数时第一次total是数组第一个参数，value是数组第二个值索引也是第二个的索引，之后的total就是上一次的返回值

```(javascript)
可以检查出现次数的函数
let arr = [1,2,3,3,4,3]
function arrayCount(array,item){
    return array.reduce((total,cur)=>{
        total += cur==item?1:0;
        return total;
    },0)
}
console.log(arrayCount(arr,3));  //3次
```


```(javascript)
let word = ["php","css"]
let string = "我喜欢在后盾人学习php与css知识"
let result = word.reduce((total,word) =>{
    return total.replace("php","javascript").replace("css","html")  //total初始值是传入的参数，之后变为上一次的返回值
},string)
console.log(result)   ///我喜欢在后盾人学习javascript与html知识
```

.forEach(function(item,key,array){},{})  //循环遍历数组,第二个参数传个对象那么this就是对象如果不传就是window。foreach可以直接操作dom节点可以不用转成数组，foreach就是隐式遍历，不能修改item值

.filter(function(value,key,array){return 条件})//过滤 挑出满足条件的值返回新数组

