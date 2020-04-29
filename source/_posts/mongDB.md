---
title: mongDB
date: 2020-04-16 20:31:50
tags:
---
## 数据库的概述
数据库可以用来永久持续存储客户端通过表单收集的用户信息
数据库软件本身可以对数据进行高校管理
通过它开放的api去操作它
database 数据库mongdb可以建立多个数据库
collection 集合一组数据的集合，可以理解为js中的数组
document 文档 一条具体的数据，可以理解为js中的对象
file 字段，文档中的属性名称，可以理解为js中的对象属性


### mac下环境变量
open -e ~/.bash_profile
添加环境变量
source ~/.bash_profile
运行配置看是否有mongod
mkdir ~/data
cd ~/data
sudo mongod --dbpath=/Users/zzx/data

### mongoose
使用nodejs操作mongdb数据库需要依赖nodejs第三方包
mongoose.connect可以让我们链接到数据库
```
mongoose.connect('mongodb://localhost/playground',{useUnifiedTopology: true ,useNewUrlParser: true})
.then(()=>{
    console.log('链接成功');
})
.catch((err)=>{
    console.log('链接失败');
})
```

## 增删查改
下面的Course User等就是带有规则的构造函数

### 添加新数据
创建集合分为两步一是对集合设定的规则，二是创建集合
mongoose.Schema设定集合规则
mongoose.model创建集合构造函数并应用规则 
创建文档实际上就是向集合中插入数据
+ 创建集合实例
+ 调用实例对象中的save方法保存到数据库
```
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/playground', { useUnifiedTopology: true, useNewUrlParser: true })
    .then(() => {
        console.log('链接成功');
    })
    .catch((err) => {
        console.log('链接失败');
    })
const courseSchema = new mongoose.Schema({  //构造函数实例化 创建规则
    name:String,
    author:String,
    isPublished:Boolean
})
//根据规则构造函数
const Course = mongoose.model('Course',courseSchema)  //数据库中名字为courses  返回构造函数有很多方法
//创建文档
const course = new Course ({  //根据集合规则构造函数创建实例
    name:'node.js基础',
    author:'张振喜',
    isPublished:true
})
//保存文档
course.save();
//第二种方式
Course.create({name:'js基础课程',author:'张振喜',isPublished:false},(err,doc)=>{
    console.log(err);
    console.log(doc);  
})
//creat返回值是promise接受then和catch
Course.create({}).then(doc=>{}).catch(err=>{})
```

### 插入现有数据
mongoimport -d 数据库名称 -c 集合名称 -file要导入文件名称
导入json数组 不用-fie 用 --jsonArray

### 查询
Course.find().then(resule=>{console.log(resule)})  //根据条件查找文档，条件为空则查找全部文档返回文档集合
Couse.findOne()then(result=>{})   //返回的是一个对象，只查找一个
find中可以写查找条件{age:{$gt:20,$lt:70}} {hobbies:{$in:["food"]}}...
.select("name")筛选字段
.skip(n)跳过n个数据
.limit(n)限制查找n
组合着用

例子：
```
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/playground',{ useUnifiedTopology: true, useNewUrlParser: true })
.then(()=>{
    console.log('success');
}).catch(err=>{
    console.log('fail'+err);
})
const userSchema = new mongoose.Schema({
    name:String,
    age:Number,
    email:String,
    password:String,
    hobbies:[String]
});
const User = mongoose.model('User',userSchema)
 User.find().then(result=>{
     console.log(result);
 })
 User.find({_id:'5e996f42e29d15a20ac0e7fa'}).then(result=>{  //返回数组
     console.log(result);
 })
User.findOne({name:"luyao"}).then(result=>{    //返回一个对象 属性name是luyao的
    console.log(result);
})
User.find({age:{$gt:20,$lt:70}}).then(result=>{   //查找age属性在20-70之间的文档
    console.log(result);
})
User.find({hobbies:{$in:["food"]}}).then(value=>{   //查找user文档中hobbies属性中包含food属性的
    console.log(value);
})
User.find().select('name email').then(result=>{  //选择要查询的字段
    console.log(result);
})
User.find().sort('age').then(result=>{   //根据age属性排序(升序) -age就是降序
    console.log(result);
})
User.find().skip(1).limit(2).then(result=>{  //skip(n)跳过n个数据，limit(n)限制n个数据
    console.log(result);
})
```

### 删除数据
Course.findOneAndDelete({}).then(result=>{}) //删除单个，删除哪个返回哪个
Course.deleteMany({}).then(result=>{})  //删除多个文档  返回要个对象{n:4,ok:1}代表删除四个ok代表删除成功
### 更新数据
Course.updataOne({查询条件},{要修改的值}).then(result=>{})  //更新一个数据
Course.updataMany({查询条件},{要修改的值}).then(result=>{})  //更新多个数据 查询条件为空代表所有文档

## mongoose验证
在创建集合规则时可以设置当前字段的验证规则，验证失败就输入验证失败
使用将验证规则放在对象中传给属性
required:true  必传字段
unique:true   传入数据库验证是否有同值的如果有同值属性，报错
minlength:2    最小长度2
maxlength:5    最大长度5
min:2 数值最小为2
max:5 数值最大为5  
default:  默认值
enum:['html','css'] 限定值只能是enum数组中的值 列出当前字段能够拥有的值
enum:{
    values:['html','css'],
    message:'自定义错误'

}
也可以写成[true,'自定义错误信息']  限制的值可以写成数组形式第一个值是限制值第二个是错误自定义信息
自定义验证规则
```
validate:{
    validator:(v)=>{
        //返回bool值true代表验证成功 v是要验证的值
        return v && v.length > 4; //验证值返回bool
    }，
    message : '自定义错误信息'
}
```

案例：
```
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/playground',{ useUnifiedTopology: true, useNewUrlParser: true })
.then(()=>{
    console.log('success');
}).catch((err)=>{err,'fail'})

const postSchema = new mongoose.Schema({
    title:{
        type:String,
        //required:true  //限制必须有这个信息
        required:[true,'标题必须填写'],
        //minlength:2,  //字符串最小长度
        //minlength:[2,'字符串长度不能小于2'],  //自定义错误信息
        //maxlength:5,   //字符串最大长度
        trim:true ,  //去除字符串前后的空格
        validate:{
            validator:v=>{
                return v && v.length>4
            },
            message:'长度必须大于4'
        }
    }
})

const Post = mongoose.model('Post',postSchema)
Post.create({
    title:'abcd'
}).then(result=>{console.log(result);
}).catch(error=>{
    //获取错误信息对象
    let err = error.errors;
    //循环错误信息对象
    for(let attr in err){
        //将错误信息打印控制台上
        console.log(err[attr]['message']);
    }
})
```
### 集合关联
用_id进行集合关联
使用populate关联集合
```
const Post = mongoose.model('Post',new mongoose.Schema({
    author:{type:mongoose.Schema.Types.objectId,ref:'User'}  //表示当前字段和user集合关联
}))
```
查询的时候
Post.find().populate('author').then((err,result)=>{console.log(result)})

通过在requset事件中对res.writeHead()书写，301是重新行为的状态码写在第一个参数，第二个参数传一个对象，location属性是重新定位的地址

### 模版引擎
art-template
require引入
const html = template('模版路径',数据)  返回的是字符串
入门用法
```
const template = require('art-template')
const path = require('path');
const views = path.join(__dirname,'views','index.html')
const html = template(views,{name:'张三',age:18})//第二个参数是要传的数据，返回拼接字符串
console.log(html);

模版文件用{{name}},{{age}}来使用传过来的数据
```

***输出***,可以在里面进行简单的运算
```
标准语法:{{数据}}    // {{a+b}}也是可以的
原始语法:<%=数据 %>   //三目<%=a?:b:c %>也是可以的 上下都可
解析传过来的数据是标签的用
{{@content}}
<%-content%>
```
***条件判断***
```
{{if 条件}} ...{{/if}}
{{if vi}}...{{else if v2}}...{{else}}...{{/if}}
<% if (value) { % >...<% } %>
<% if(v1) { %> ... <%} else if(v2) {%> ... <% } % > 
```
***循环***
```
{{each 数据}}...{{/each}}   //{{$index}}是循环当前数组中的index {{$value}}是循环当前数组中的值
<% for(var i = 0; i< target.length;i++) {%>
<% =i %> <%=target[i]%>  
<%}%>
```
***子模版***
```
使用子模版可以将网站的公共区块抽离到单独的文件中
标准语法:{{include '模版路径'}}
原始语法:<%include('模版路径')%>
用法就是把共同部分抽离出来放到一个单独的文件中
用上面的方式在模版中引入
```

***模版继承***
```
使用模版继承可以将网站html骨架抽离到单独的文件中，其他页面模版可以继承骨架文件
{{block 'head'}} {{/block}}  预留位置，写在模版中block后面是预留的位置名称
{{extend 'path'}}来继承模版骨架

***模版配置***
向模版中导入变量 template.default.imports.变量名 = 变量值
dateformat时间格式化的第三方模块
```
//导入模版变量
```
template.defaults.imports.dateFormat = dateFormat
const path1 = path.join(__dirname,'views','extend.html')
console.log(path1)
const html = template(path1,{
    time:new Date()
})
console.log(html);
//在模版中直接用导出的dateFormat
```

设置模版的根目录
template.default.root = 模版目录
当你template('path',{})时
path可以直接写渲染模版文件的名字，会在设置的模版根目录里找对应的模版文件
配置模版的默认后缀
template.default.extname = '.html'
这样你输入文件名的时候后缀就可以省略了
```
template.defaults.extname = '.html'
template.defaults.imports.dateFormat = dateFormat
template.defaults.root = path.join(__dirname,'views')
const html1 = template('extend',{
    time:new Date()
})
const html2 = template('index',{name:'张三',age:18,content:`<p>hhh</p>`,msg:'我是模版'})//第二个参数是要传的数据，返回拼接字符串

console.log(html1);
console.log('-------------------------------------------------------');
console.log(html2);
```
***router第三方模块***
实现路由
获取路由对象
调用路由对象提供的方法创建路由
启用路由让路由生效
```
const getRouter = require('router')
const router = getRouter()
router.get('/pathname',(req,res)=>{})
router.post('/pathname',(req,res)=>{})
server.on('request',(req,res)=>{
    router(req,res)   //在服务对象中调用路由对象
})
```

***serve-static***
实现静态资源访问服务 
主要思想就是将css等文件放在静态资源访问目录里当浏览器想要获取css样式会触发请求这时候回去public文件夹中寻找引入
引入得到方法
通过创建静态资源服务并制定资源服务目录
启用静态资源访问服务

```
const serveStatic = require('serve-static')
const serve = serveStatic(绝对路径的public文件夹)
server.on('request',(req,res)=>{
    serve(req,res)    //在服务对象中调用静态资源访问服务
})
```
