---
title: js笔记8
date: 2020-03-31 10:24:38
tags: 
    -js
    -学习笔记
categories: 前端
---

## JSON
键值对，键用双引号包起来
数组和对象都可以转为json
JOSN.stringify(obj,["property"],tab)  //转为json格式，是字符串类型,第二个参数就是保留那些属性在[]里写上，null则代表全部保留,tab是制表位
JSON.parse(obj,(key,value)=>{return key or value })   //转换成对象,可以对解析之后的进行value和key的操作，返回想修改的东西。
在对象中可以自定义toJSON方法，当JSON.stringify时就返回定制的返回值 
```
let obj = {
    name:"zzx",
    age:18,
    teacher:{
        name:"xj"
    },
    toJSON:functioin(){
        return {
            name:this.name,
            teacher:this.teacher.name
        }
    }
}
console.log(JSON.stringify(obj))  //用toJSON的返回值
```

## 原型
Object.getPrototypeOf(obj)  //查看原型父级
Object.getOwnPropertyNames(u)  //打印对象的属性名
Object.setPrototypeOf(a,b)  //给a对象设置原型b，b是a的原型
Object.create(father,{})   //创建个对象并指定原型并返回这个对象,可以创建没有原型的对象，father写成null就行
a.isPrototypeOf(b)  //a是否是在b的原型链上,a是否是b的原型

.__proto__  //查看obj的原型
可以给原型加属性和方法 obj.__proto__.fun =function(){}
.prototype  //原型对象  一般服务与构造函数实例化使用的，一般是在构造函数中有这个的
```
function User(){
    name:"zzx",
    age:18
}
let user = new User()
User.__proto__  //给函数本身用的比如说里面有apply方法
User.prototype.show = function(){console.log(this.name)} //当函数作为构造函数实例化出来对象的时候的原型方法
user.show();
```
Object.prototype是最顶级的
arr.__proto__ == Array.prototype 
给构造函数的prototype加函数就相当于给实例化对象的__proto__加函数，每一个实例化对象都能调用
```
function User(name){
    this.name= name
}
User.prototype.show =function(){   //给构造函数原形添加函数
    console.log(this.name)
}
User.prototype = {   //改变user构造函数的原型
    constructor:User,   //记得加上构造函数，因为之前的函数就有，可通过构造函数User的原型来new对象
    show(){
        console.log(this.name)
    }
}
```

根据对象创建新的对象
```
function User(name){
        this.name=name,
        this.show=function(){
            console.log(this.name)
        }
    }
    let hd = new User("houdunren");
    console.log(hd);
    
    function creatByObject(obj,...args){
        const constructor = Object.getPrototypeOf(obj).constructor;  //的保证原型的constructor存在
        return new constructor(...args);
    };
    let xj = new creatByObject(hd,"xj");
    xj.show()
    
```
instanceof关键字
```
console.log(a instanceof A)  //a的原型链上是否有A的prototype
```

遍历对象的时候for in 如果这个对象有原型，不想把原型属性遍历出来要加判断条件
```
for(const key in obj){   //in能检测出原型链中的属性
    if(obj.hasOwnproperty(key)){
        //操作
    }
}
```

合理的构造函数方法声明  
```
function User(name){
    this.name = name
}
User.prototype.show = function(){  //在原型中添加属性节省内存空间
    console.log(this.name);       //或者把原型改变为一个新的对象里面写出方法
}
let lisi = new User("lisi");
let zs = new User("zs");
lisi.show();
zs.show();
```

Object.create(obj,{})
`let hd = Object.create(User,{name:"zzx"})  //将user设置为hd原型，并且向hd中加属性`


.__protot__不是严格的属性，是原型中有get和set访问设置和获取的
想设置某个对象又一个叫做__protot__的属性不想设置成原型，那么用let hd = Object.create(null),
这样就可以设置了，hd没有原型就不会有set proto


### 继承
js没有多继承
继承不改变原型

构造函数a想继承构造函数b
`a.prototype.__proto__ ==b.prototype`
或者
`a.prototype = Object.create(b.prototype)  //创建一个原型是b的原型的对象作为a的原型`
下面的这种方式注意了如果在修改之前实例化的对象在修改之后原型并没有改变，且a的原型的constructor改变了
a.prototype.constructor = a;需要禁用遍历用Object.definePrototype()

### 多态
根据多种不同的形态产生不同的结果，下而会根据不同形态的对象得到了不同的结果。
使用父类构造函数来初始属性
```
function User(name,age){
    this.name= name;
    this.age = age;
}
User.prototype.show = function(){
    console.log(this.name,this.age)
}
function Admin(name,age){
    User.call(this,name,age);   //调用User里面的this应该用call改变
}
Admin.prototype = Object.creat(User.prototype);
let zzx = new Admin("向军",18)
```

### mixin super

this.__proto__ 可以用super代替
JS不能实现多继承，如果要使用多个类的方法时可以使用mixin混合模式来完成。
mixin 类是一个包含许多供其它类使用的方法的类
Object.assign(Admin.prototype, Request, Credit);

## 类
calss 实际上就是函数

```
class User{  
    site="baidu"
    constructor(name){
        this.name = name;

    }
    show(){     //创建对象之后就自动把函数放在原型里了,且不会被遍历出来
        console.log(this.name)
    }
}
```

静态属性
static 定义 通过类名.静态属性名，静态属性是给所有类对象共用的
```
class Request{
    static host = "https://baidu.com"
    api(){
        return Request.host + "/url"
    }
}
```
静态方法
函数作为对象直接添加属性就是函数的静态方法
static定义 用类名.函数名()直接调用，是所有对象可以共用的，可以直接调用
类中也有访问器
类中属性可以用个对象接住属性保护起来

使用命名规则保护属性
以_开头的属性代表私有属性就不要用这个属性去修改_url就是私有的尽量不要去修改她而是用url去修改，在类中写set对这个进行验证

保护属性
1.用Symbol
```
const HOST = Symbol("主机");
class User{
    [HOST] = "www.baidu.com";   //外部不能随意修改设置set可以规定修改格式
    get host(){
        return this[HOST];
    }

}
```
类继承 子类也可以使用私有属性
extends
下面使用 Symbol定义私有访问属性，即在外部通过查看对象结构无法获取的属性
```
const protecteds = Symbol();
class Common {
  constructor() {
    this[protecteds] = {};   //将对象的属性保护起来
    this[protecteds].host = "https://houdunren.com";
  }
  set host(url) {
    if (!/^https?:/i.test(url)) {
      throw new Error("非常网址");
    }
    this[protecteds].host = url;
  }
  get host() {
    return this[protecteds].host;
  }
}
class User extends Common {
  constructor(name) {
    super();
    this[protecteds].name = name;
  }
  get name() {
    return this[protecteds].name;
  }
}
let hd = new User("后盾人");
hd.host = "https://www.hdcms.com";
// console.log(hd[Symbol()]);
console.log(hd.name);

```


WeakMap
WeakMap 是一组键/值对的集，下面利用WeakMap类型特性定义私有属性
```
const _host = new WeakMap();
class Common {
  constructor() {
    _host.set(this, "https://houdunren.com");
  }
  set host(url) {
    if (!/^https:\/\//i.test(url)) {
      throw new Error("网址错误");
    }
    _host.set(this, url);
  }
}
class Article extends Common {
  constructor() {
    super();
  }
  lists() {
    return `${_host.get(this)}/article`;
  }
}
let article = new Article();
console.log(article.lists()); //https://houdunren.com/article
article.host = "https://hdcms.com";
console.log(article.lists()); //https://hdcms.com/article

```

私有属性
#name
在类外部不能修改，内部可以修改，获取，可以通过访问器修改和获取
私有方法也是同样定义 在外部不可以访问，不能继承
```
#check = ()=>{}
```

子类构造函数中必须要用super()来调用父类的构造函数
继承就是a继承b，a的原型继承b的原型
子类调用父类的方法用super.method()
静态方法和属性可以继
