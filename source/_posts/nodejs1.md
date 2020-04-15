---
title: nodejs
date: 2020-04-11 22:55:11
tags:

    -nodejs
    

---

## node

node 不是独立的语言， 是一个基于chorme v8 引擎的js运行环境，让js运行在操作系统中
双击node.exe 或者用浏览器console控制台或者vscode终端，cmd等等打开并进入repl环境可以运行js代码

### fs模块

const fs = require("fs")  //引入fs模块，用fs.api来调用api，fs就是操作文件系统的api
fs.readFile(url, encode, function(err, data){})  buffer是一个二进制数据流，不写encode的话用data.toString('utf8')也能解析中文
fs.writeFile()//覆盖写入，无则创建有则覆盖
fs.appendFile()//添加写入
fs.unlink()//删除文件

fs.readdir(path, options, callback)读取path文件目录
在回调函数中第一个参数错误优先
c(create)u(update)r(read)d(del)
fs.mkdir(path, [, module], callback)创建文件夹
fs.rename(oldpath, newpath, callback)//修改文件名
fs.rmdir(path, callback)//删除空文件夹
Class:fs. Stats  //fs的类
fs.stat('url', (err, stats)=>{

    stats.isFile()  //判断检查的文件是否是文件

})

### url模块

``` 
const url = require('url');
url.parse(urlString)  //将字符串变为url对象
url.format(obj)     //将url对象转换成字符串
```

### querystring模块

解析url的查询字符串

``` 
let qs = require('querystring')
qs.parse(string,["&"],["="])  将query字符串(name=zzx&pass=123)转换成,以&区分键值对，=区分键和值
qs.stringify(string,["&"],["="]) 将对象拼接成query字符串
qs.escape(string)  //给字符串编码
qs.unescape(string)  //给字符串解码
```

### 自定义模块

``` 
一个js就是一个模块
let name = {
    say(){
        console.log("hello")
    }
}
module.exports = name  //导出
let module = require('./moudlename')  //引入自定义模块
```

### 第三方模块

邮箱验证
 + Nodemailer 可以实现发邮件

### Error对象

``` 
let err = new Error("发生错误") //这是一个错误对象，
throw err //抛出错误，终止执行
console.log(err)  //打印错误
```

### node爬虫案例  http模块

1. 获取网站  http.get
2. 分析内容  cheerio  可以使用jq里的选择器, 将字符串转换为类dom元素之后就能用过jq来获取元素内容
3. 获取有效信息，进行下载或者其他操作

``` 
// 请求网站数据，将数据保存本地
const http = require('https')
let url = 'https://www.qunar.com'
const fs = require("fs");
const ci = require("cheerio")
http.get(url, (res) => {
    //安全判断
    const { statusCode } = res;  //状态码
    const contentType = res.headers['content-type']  //文件类型
    // console.log(statusCode,contentType);
    let error = null;
    if (statusCode !== 200) { error = new Error("请求状态吗错误") } else if (!/^text\/html/.test(contentType)) {
        error = new Error("类型错误")
    }
    if (error) {
        console.log(error);
        res.resume();  //重置缓存
        return false
    }
    let rawData = '';
    //数据分段，只要接受数据就会出发data事件，chunk就是每次接受的数据片段
    res.on('data', (chunk) => {    //on就是监听事件函数
        console.log("数据传输-------");
        rawData += chunk.toString('utf8')
        // console.log(chunk.toString('utf8'));
    })
    //end是数据流传输完毕
    res.on('end', () => {
        console.log('数据传输完毕');
        //cheerio分析
        let $ = ci.load(rawData);
        $("img").each((index, value)=>{
            console.log($(value).attr('src'))
        })
        //将请求的数据保存到本地
        // fs.writeFile('./baidu.html', rawData, (err) => {
        //     console.log(err);

        // })
    })

}).on("error", (err) => {
    console.log("请求错误" + err);
})
```

### 通过express 框架书写api

登陆接口逻辑分析

1. 接受数据
2. 处理数据
3. 返回数据

#### express 基本使用

1. 安装

``` 
npm install express --save
```

模块的安装引用先从当前目录的node-module找后以此向上级找知道找不到

2. 开启服务

``` 
const express = require("express");
const app = express();
app.listen(3000,()=>{   //开启服务
    console.log('strat server');
})
app.get('/user/login',(req,res)=>{   //pathname要语义化
    let{name,age}=req.query  //请求的参数query，可以对传过来的参数进行操作验证,结构赋值
    res.send({err:0,text:'信息'})  //返回的信息
})
```

书写api案例

``` 
const express = require('express');
const app = express();//express实例化
const bodyParser = require("body-parser")
// parse application/x-www-form-urlencoded 解析form表单数据 参数键值对
app.use(bodyParser.urlencoded({ extended: false })) //app.use表示使用中间件(插件)

// parse application/json 解析json样式的参数
app.use(bodyParser.json())
//最简单的API接口
app.get('/user/login', (req, res) => {   //req请求，res响应
    //get接受参数query,由后端确定参数名字
    console.log(req.query);  //请求的query，键值对？后面的
    //处理参数
    let { name } = req.query;
    if (name == "zzx") {
        res.send({ err: 0, msg: 'regist ok' })
    } else {
        res.send({ err: 0, msg: 'regist no ok' })
    }
    console.log('你好');
})
app.post('/user/login', (req, res) => {
    //post数据放在请求体和消息体中req.body
    //express不能直接解析消息体，需要通过第三方插件来解析body-parser
    console.log(req.body);  
    let {us,ps}=req.body
    if(us==123&&ps==123){
    res.send({err:0,regist:'ok'})
    }else{
        res.send({err:-1,regist:'no ok'})
    }

})
app.listen(3000, () => {  //监听3000端口号,已经开启了服务器
    console.log('server start');

})
//http://localhost:3000/user/login
//192.168.31.106:3000/user/login  都可以访问
```

postman 工具 接口测试
中间件 middlewear

* 内置中间件static
* 自定义中间件(全局和局部)
* 第三方中间件(body-parser) (拦截器)

自定义：
全局
app.use('/url', (req, res, next){next()}) 在进行访问/后面的文件时优先进行中间件可以在中间件中进行验证是否含有query，如果url是根目录app.use('/', ()=>{}) == app.use(function(){})，next()使代码继续执行
局部
app.get('/pathname', (req, res, next)=>{

    console.log('局部中间件)
    next();

}, (req, res)=>{

    console.log('执行体')

})

static静态资源目录
app.use(express.static(path.join(__dirname, name)))   //创建静态目录
//域名：端口号直接指向静态目录（path默认为'/'）
app.use('/public', express.static(path.join(__dirname, name)))
域名：端口号/public才是静态目录

#### api接口书写

* 接口书写
  + get req.query
  + post req.body 需要body-parser 插件进行解析 
  + postman进行测试
    - 注意数据格式 json x-www-form-urencode formdata

     - app.use(bodyParser.urlencoded({ extended: false }))  x-www-form-urencode
     - app.use(bodyParser.json())  json

#### 服务器相关

服务器就是一台电脑
服务器软件启动服务，apache(php) tomcat(java) ils(Mircosoft) ngnix(linux) node(js)
服务器ip和端口号 端口号默认80, ip服务器主机位置，端口号确定服务器某个应用程序

#### 路由

路由的作用就是防止所有接口都写在入口文件，所以用express中的router实例化路由通过路由写接口
之后导出路由在接口文件导入用中间件来引用

``` 
server.js

const express = require("express")
const app = express();
const userrouter = require('./router/router')  //引用路由模块
app.use('/user',userrouter)//在/user目录下引用路由，由路由处理接口
app.listen(3003,()=>{
    console.log("server start");
})

router.js
const express = require('express') 创建路由实例，路由的作用就是防止我们所有的接口都写在入口文件
router.get('/add',(req,res)=>{
    res.send('user add')
})
router.get('/del',(req,res)=>{
    res.send('user del')
})

module.exports = router;  //将实例化的对象抛出去
```

### 非关系数据库 mongdb

* 安装

  -下载官网

