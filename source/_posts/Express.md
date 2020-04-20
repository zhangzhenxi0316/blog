---
title: Express
date: 2020-04-20 13:44:09
tags:
---

+ 简洁的路由定义方式
+ 对过去http请求参数进行了简化处理
+ 对模版引擎支持度高
+ 提供中间件机制有效控制http请求
+ 拥有大量第三方中间件对功能进行扩展

入门代码
```
const express = require('express') //引入模块
const app = express()   //创建http服务
app.listen(3000,()=>{    //监听端口
    console.log('server success');
})
//创建路由
app.get('/',(req,res)=>{
    //send对客户端进行响应
    //1.send方法会自动检测文件的内容类型
    //2.send方法会自动设置http状态码
    //3.send方法会帮我们自动设置响应内容及编码
    res.send({name:'zzx',age:20})
})
```
## 中间件
中间件就是一堆方法，可以接受客户端发送过来的请求对请求作出响应，也可以继续交给下一个中间件继续处理
我认为中间件就是一个拦截在路由前面的一个路由，可以对请求作出判断之后在抛给下一个中间件或者路由继续作出回应
中间件只要由两部分构成，中间件方法和时间处理函数
中间件方法由express提供，负责拦截请求请求处理函数由开发人员提供，负责处理请求
对一个请求可以设置多个中间件，进行多次处理
默认情况下请求由上到下执行一旦匹配一个中间件就终止匹配了，用**next**方法将请求控制权交给下一个中间件
中间件不传pathname那么代表'/'根目录
+ get中间件
  - app.get(pathname,(req,res,next)=>{})
+ post中间件
 - app.post(pathname,(req,res,next)=>{})
+ 不分请求方式的中间件
 - app.use(pathname,(req,res,next)=>{})

### app.use
 app.use方法可以接收所有路径下的请求也可以指定某一路径下的请求
 中间件的应用
 1.路由保护，客户端进行需要登陆的页面时可以先使用中间件判断用户的登陆状态，用户未登陆就拦截请求作出响应
 2.网站维护公告，在所有路由的最上面定义接受所有请求的中间件，直接为客户端作出响应，定义在所有路由最前面
 3.自定义404页面  定义在所有路由的最后面res.status(404).send('访问不存在')

#### 错误处理中间件
 在程序执行的过程中不可避免的会出现一些无法预料的错误，比如文件读取失败，数据库链接失败
 错误处理中间件是一个集中处理错误的地方
 app.use((err,req,res,next)=>{})  当程序发生未知错误时会调用这个函数,只能捕获同步代码中的错误，如果想捕捉异步错误那么就在异步API的回调中用next抛出来
 ```
const express = require('express')
const fs = require('fs')

const app = express()
app.listen(3000,()=>{console.log('serve success')})
app.get('/file',(req,res,next)=>{
    fs.readFile('/3.txt',(err,data)=>{
        if(err){
            next(err);
        }
    })
})
app.get('/list',(req,res)=>{
    throw new Error('程序发生未知错误')
})
app.use((err,req,res,next)=>{
    res.status(500).send('服务器出现问题:'+err.message)
})
 ```

 异步api错误信息通过回调函数，异步promise错误通过catch捕捉
 异步函数执行错误通过try catch 在catch中next抛出
```
const promisify = require('util').promisify;
const readFile = promisify(fs.readFile)
app.get('/file',async(req,res,next)=>{
    try{
    await readFile('/4.txt')
    }catch(err){
        next(err)
    }
})
app.use((err,req,res,next)=>{
    res.status(500).send('服务器出现问题:'+err.message)
})
```

## 模块化路由
```
入口文件
const express = require('express')
const app = express();
const index = require('./route/index')//导入二级路由
const admin = require('./route/admin')
app.listen(3000)
app.use('/home',index);//一级路由
app.use('/admin',admin);

index.js
const express = require('express')
const app = express();
const index = express.Router()
index.get('/index',(req,res)=>{  //二级路由
    res.send('欢迎来到博客')
})
module.exports = index

admin.js
const express = require('express')
const app = express();
const admin = express.Router()
admin.get('/index',(req,res)=>{  //二级路由
    res.send('欢迎来到用户中心')
})
module.exports = admin

通过localhost:3000/home/index和localhost:3000/admin/index访问到两个页面
```

## GET参数的获取
使用req.query获取get参数返回对象

## POST参数获取
express中接收post参数需要借助第三方包body-parser
```
const express = require('express')
const bodyParser = require('body-parser')
const app = express()
//用中间件拦截，去解析body
app.use(bodyParser.urlencoded({extended:false}))//extended代表用哪个模块解析系统参数false用系统模块querystring，true用第三方模块解析
app.post('/add',(req,res)=>{
    res.send(req.body)// post请求参数
})
app.listen(3000)
```

## 路由参数
不传参数路由不能链接到路由中，可以设置多个路由参数
```
const express = require('express')
const app = express();
//get中/:id就是路由参数 
app.get('/index/:id',(req,res)=>{//需要接受id作为参数  /index/123不传路由参数不能链接
    res.send('链接成功')
})
app.listen(3000)
```

## 静态资源访问
通过express内置的express.static可以方便的托管静态文件 如img css js
app.use(express.static('public'))//可以传第一个参数一个路径，那么访问时就要加上那个路径，什么都不加用localhost:3000/default.html就能访问

## 模版引擎
express在原有的art-template模版引擎中封装了express-art-template
需要同时安装atr-template和express-art-template
app.engine('art',require('express-art-template')) 使用express-art-template模版来渲染后缀是art的文件
app.set('views',path.join(__dirname,'views'))设置模版存放目录
app.set('view engine','art')渲染模版时不写后缀，默认拼接art后缀
app.get('/',(req,res)=>{
    res.render('index') 第一个参数是模版名称，
})

```
const express  = require('express')
const path = require('path')
const app = express()
//告诉express框架使用什么模版引擎渲染什么后缀的模版文件
//1.模版后缀2.使用的模版引擎
app.engine('html',require('express-art-template'))
//告诉express框架模版存放的位置
//1.是express的配置项名字views，第二个才是路径
app.set('views',path.join(__dirname,'views'))
//告诉express框架模版的默认后缀是
//1.也是配置项名字2.默认后缀
app.set('view engine','html')

app.get('/index',(req,res)=>{
    //渲染模版 用express中的res.render 方法渲染模版1.模版名字2.传入模版的变量
    res.render('index',{  //1.拼接模版路径2.拼接模版后缀3.哪个模版和哪个数据进行拼接4.将结果返回客户端
        msg:'index界面'
    })
})
app.get('/list',(req,res)=>{
    res.render('list',{
        msg:'list界面'
    })
})
```

**app.locals对象**
将变量设置到app.locals对象下面，这个数据在所有的模版中都可以获取到
```
app.locals.users=[{name:'zzx',age:20},{name:'李四',age:30}]
//所有模版都可以拿到该属性
 <ul>
        {{each users}}
            <li>
                {{$value.name}}
                {{$value.age}}
            </li>
        {{/each}}
    </ul>
```
 

