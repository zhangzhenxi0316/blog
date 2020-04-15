---
title: js笔记7
date: 2020-03-30 13:08:36
tags:
    - js
    - 学习笔记
---
## 闭包
```
函数可以访问到其他函数的数据叫做闭包
let btns = document.querySelectorAll("button");
btns.forEach(function(item){
    let left = 1;    //如果left放在click点击时间内难么每一次点击都会创建一个环境环境里面的left都会是单独存在的，所以要放在外面
    let interval = false;  //节流阀概念，不让定时器多次开启
    item.addEventListener("click",function(){
        if(!interval){
            interval = true;
        setInterval(function(){
            item.style.left = left++ +"px";
        },100)
        }
    })
})
```

优化内存的方案解决闭包带来的内存泄漏问题
```let divs = document.querySelectorAll("div");
divs.forEach(function(item){
    let desc = item.getAttribute("desc");  在外部将需要的值保存下来，内部打印值就好，不要
    item.addEventListener("click",function(){  //每次点击都会创建一个内存环境这个匿名函数被引用所以外部的div也会保存在环境中
        console.log(desc);
        //console.log(item.getAttribute("desc"))  //这样做导致内存泄漏，增加缓存
        console.log(item);
    });
    item = null;  //将item变为空这样虽然每次点击会创建环境但是div这个对象在这里清空了将不会保存
})
```
### this在闭包中的遗留问题

```
let hd = {
    user: "houdunren"
    get:function(){
        return function(){  //这里用箭头函数可以访问到父级的this就不会在下面指向调用函数的对象，或者在上面定义一个常量接受this
            return this.user;
        };
    }
};
console.log(hd.get())  //hd.get()返回函数
console.log(hd.get()())   //this指向window因为相当于let a = hd();a(),相当于在window调用函数所以指向window
```

## 对象
通过直接添加可以追加属性user.name = "zzx";
通过delete可以删除属性 deltet user.name;
对象是引用类型
对象也可以用展开语法
```
let obj1 ={age:18,name:"zzx"}
let obj2 = {...obj1,lang:"zh"}
console.log(obj2)  //{age:18,name:"zzx",lang:"zh"}
```
对象中重名属性后面值会覆盖前面的值
```
let obj1={
    name:"zzx",
    age:18;
}
let obj2 = {
    name:"zzs"
    lang:"zh"
}
console.log({...obj1,...obj2})  //{name:"zzs",age:18,lang:"zh"}
```
结构赋值 不能省略声明变量 结构赋值可以有默认值
```
let user = {name:"houdunren",age:18};
let {name1:n,age1:a title="javascript"}=user //n就是houdunren  a就是18
let {name,age}=user  //结构赋值取出name和age 
```
传参数也可用结构赋值 参数可以不用全部接收
```
funtion user({name,age}){
    console.log(name,age);
}
user({name:"zzx",age:18})
```
结构默认值实现配置项合并
```
function creatElement(options ={}){
    let {width=200,height=100,backgroundColor="red"}=options;
    const div = document.createElement("div");
    div.style.width = width +"px";
    div.style.height = height +"px";
    div.style.backgroundColor = backgroundColor;
    document.body.appendChild(div)
}
creatElement({width:60,height:30});
```

可以部分用结构特性接受参数
```
function hd(user,{name,age}){}
```

### 属性
obj.name="zzx" //添加修改属性
obj["name"] = "zzx"  //添加修改属性，如果属性值是变量那么就用[]来增删改查
.hasOwnProperty("属性名")  //查看对象是否含有属性名返回布尔值,不看原型是否有改属性
in 关键字就能检查原型链或者自身是否含有该属性
`console.log("concat" in arr)`
Object.assign(obj1,obj2)  //方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
Object.setPrototypeOf(a,b)  //给a对象设置原型b

```
let a ={name:"zzx"}
let b = {age:18}
Object.setPrototypeOf(a,b) //将b设置为a的原型
console.log(a.hasOwnProperty("age"))  //false
console.log("age" in a)  //true
```

案例:动态改变数据属性，将数组变为对象并前面加上编号属性
```
let a = [
         {title:"js",
         categories:"语言"},

         {title:"js",
         categories:"语言"},

         {title:"js",
         categories:"语言"}
     ]
     let res = a.reduce((obj,cur,index)=>{
         obj[`${cur.categories}-${index}`] = cur;
         return obj;
     },{})
     console.log(JSON.stringify(res,null,2))  //JSON
```
Object.keys(obj)  //获取对象所有的健名返回数组
Object.values(obj)  //获取对象所有的值
Object.entries(obj)  //获取对象所有的键值对返回二维数组
对象可以for in遍历对象 但是不能for of迭代对象，但是可以迭代Object.keys()等
for(const [key,value] of Object.entries(obj)){}  //结合结构赋值特性和迭代遍历对象键值对

对象浅拷贝
let obj1 = {...obj};
let obj2 = Object.assign(obj3,obj4)
对象深拷贝
```
自定义函数实现深拷贝
function copy(object) {
    let res = object instanceof Array? []:{};
    for(let [k,v] of Object.entries(obj)){
        res[k] = type of v == 'object' ? copy(object[k]):v;
    }
    return res;
}
let hd = copy(data);  //对data进行深拷贝
console.log(hd) 
```
### 工厂函数创建对象
```
function User(name){
    return {
        name,
        show:function(){
            console.log(this.name)
        }
    }
}
let lisi = User("李四");
```
### 构造函数方式建立对象
```
function User(name){
    this.name = name;
    this.show = function(){
        console.log(this.name)
    };
    //return this;  //返回当前对象，不写系统也会自动帮我们返回
}
let zzx = new User("张振喜")
console.log(zzx)  //可以打印当前对象
zzx.show()  //调用方法
let fun = zzx.show
fun()  //fun方法里面的this是window
```

### 抽象与封装
对属性抽象化处理
```
function User(name,age){
    let data = {name,age}   //对内部数据进行抽象封装，这是省略写法属性名和变量名重名可简写实际上是{name:name,age:age}
    this.show= function(){
        console.log(this.name,this.age +this.info())
    }
    let info = function(){     //方法进行抽象，在外部不能访问并修改
        return data.age>50?"老年":"青年";
    };
}
```

### 属性特性
Object.getOwnPropertyDescriptor(obj,"property")//查看对象中某个属性的特性
Object.getOwnPropertyDescriptors(obj)  //显示所有属性
```
let user = {
         name:"张振喜",
         age:18
     }
console.log(JSON.stringify(Object.getOwnPropertyDescriptor(user,"name")))
//{"value":"张振喜","writable":true,"enumerable":true,"configurable":true}
//value值 enumerable可枚举遍历 writable可写 configurable可删除这个属性是单行的改了就不能变回来
Object.defineProperty(user,"name",{
         value:"hhh", 
         writable:false
     })
```
下面的方式可以修改属性特性，也可以添加没有的属性
Object.defineProperty(obj,"property",{
    value:"value"
    ...
})
Object.defineProperties(obj,{   //复数形式就是批量设置特性
    name:{
        value:"houdunren"
        ...
    }
    age:{
        ...
    }
})
Object.preventExtensions(obj)  //禁止向对象中添加属性
Object.isExtensible(obj)  //检测是否可以在对象中添加属性返回bool值
Object.seal(obj)  //不能添加对象属性也不能删除，封闭对象 封闭对象
Object.isSealed(obj)  //检测对象是否处于封闭状态
Object.freeze(obj)  //不能修改添加删除属性，冻结对象
Object.isForzen(obj)  //检测是否对象冻结了
使用访问器来保护属性
```
const user ={
    //name和age不会被随意修改，用法也没有发生变化
    data :{name:"houdunren",age:19},
    set age(value){     //访问器其实定义成方法也有同样效果，用set可以用=来传参数
        if(typeof value != "number" || value <10 || value > 100){
            throw new Error("年龄格式错误")
        }
    },
    get age(){return this.data.age + 岁}  //访问器  可以直接输出 ,
}
user.age = 999;
console.log(user.age)
```

使用访问器批量设置属性 访问器属性高
```
let web = {
    data:{name}  //定义成私有属性 没有访问函数
    age:18,
    set site(){
        [this name ,this.url] = value.split(",");
    }
    get name(){
        return this.data.name
    }
}
web.site = "百度,www.baidu.com"
console.log(web.name)   //百度
console.log(web.url)   //www.baidu.com
```
构造函数中通过defineProperties给属性规定set和get
```
function user(name,age){
    let data = [name,age];
    Object.defineProperties(this,{
        name:{
            get(){
                return data.name;
            }
            set(value){
                data.name=value;
            }
        }
    })
}
```

### 对象的代理Proxy
```
let hd = {name:"zzx",age:18};
const proxy = new Proxy(hd,{   //代理hd对象
    get(obj,property){
        return obj[property];
    },
    set(obj,property,value){
        obj[proerty] = value;
        return true;
    }
})
console.log(proxy.name);
proxy.name = "baidu";
console.log(proxy.name)

```
### 代理方法控制函数
```
function factorial(num){
    return num==1?1:num*factorial(num-1);
}
let proxy  = new Proxy(factorial,{
    apply(func,obj,args){
        console.time('run')
        func.apply(this,args);
        console.timeEnd('run);
    }
})
```
### 代理对数组的拦截
```
let proxy = new Proxy(arr,{
    get(array,key){
        return array[key]
    }
})
console.log(proxy[key])
```

代理案例双向实现同步
```
<body>
    <input type="text" v-model="title">
    <input type="text" v-model="title">
    <div v-bind="title"></div>
    <script>
       function Viwe(){
           let proxy = new Proxy({},{
               get(obj,property){
                    return obj[property];
               },
               set(obj,property,value){
                   document.querySelectorAll(`[v-model="${property}"]`).forEach(item=>{
                       item.value = value;
                   })
                   document.querySelectorAll(`[v-bind="${property}"]`).forEach(item=>{
                       item.innerHTML = value;
                   })
                   return true;
               }
           });
            this.init =function(){
               let input = document.querySelectorAll("[v-model]").forEach(item=>{
                   item.addEventListener("keyup",function(){
                        proxy[this.getAttribute("v-model")] = this.value;
                   })
               })
           }
       };
       
       new Viwe().init();
    </script>
</body>
```
<!-- more -->

代理模拟表单验证
```
<input type="text" verification rule="max:8,min:1">
    <input type="text" verification rule="max:8,isNumber">
    <div v-bind="title"></div>
    <script>
        class val{
            max(value,len){
                return value.length<len;
            }
            min(value,len){
                return value.length>len;
            }
            isNumber(value){
                return /^\d+$/.text(value);
            }
        };
       function ProxyFactory(obj){
        return new Proxy(obj,{
            get(obj,property){
                return obj[property]
            },
            set(obj,property,el){
                const rule = el.getAttribute("rule");
                const vall = new val();
                let state = rule.split(",").every(rule=>{
                    const info = rule.split(":");
                    return vall[info[0]](el.value,info[1]);
                })
                el.classList[state?"remove":"add"]("error");
                return true;
            }
        })
       };
       let proxy = ProxyFactory(document.querySelectorAll("[verification]"))
       proxy.forEach((item,i)=>{
           item.addEventListener("keyup",function(){
            proxy[i] = this;
           })
       })
```



