---
title: http与promise
date: 2020-04-16 20:29:58
tags:
    - nodejs
categories: Nodejs 
---

### 服务器端
默认80端口
127.0.0.1 == localhost
创建web服务器用http模块
```
const http = require('http') //引用http模块
const app = http.createServer()是创建网站服务器
app.on('request',(req,res)=>{res.end("")})  //响应  
app.listen(3000)  //监听3000端口
```
req.method可以查看请求方式GET OR POST
req.url 请求地址
req.headers 请求报文

响应报文
http状态码
res.writeHead(500);//写响应报文，第一个参数就是响应码
内容类型
res.writeHead(200,{
        'content-type' : 'text/html;charset=utf-8'
    })
url.parse(req.url)//解析请求的url变为对象
url.parse(req.url,true)//将查询参数解析成对象的形式
let {query,pathname} = url.parse(req.url,true)  //取出解析完的参数和路径,url模块
querystring模块可以对参数字符串进行解析querystring.parse()//将参数解析成对象模式

总结
获取get参数用req.url就行用url.parse或者querystring都行
过去post不能直接获取用req的data和end事件里面所传的参数就是参数用querystring。parse解析
```
//post参数获取
let postParams = '';
    req.on('data',(params)=>{
        console.log(params.toString());
        
        postParams += params;
    })
    req.on('end',()=>{
        console.log(querystring.parse(postParams));
        
    })
    res.end('ok')
```
### 路由
http中的路由就是对传进来的参数进行解析判断进行不同的响应，
### 静态资源
客户端不需要处理可以直接给客户端的资源就是静态资源

```
静态资源访问，只需要在地址栏中输入localhost:3000/default.html就能访问静态资源目录下的default.html不需要输入其他路径
const http = require('http')
const app = http.createServer()
const url = require('url')
const path = require('path')
const fs = require('fs')
const mime = require('mime')   //根据资源判断类型
app.on('request',(req,res)=>{
    let pathname = url.parse(req.url).pathname
    pathname = pathname =='/'?'/default.html':pathname;
    let realpath = path.join(__dirname,'./public'+pathname)
    let type = mime.getType(realpath);
    console.log(type);
    fs.readFile(realpath,(err,result)=>{
        if(err !=null){
            res.writeHead(404,{
                'content-type':'text/html;charset=utf-8'
            })
            res.end('fail失败')
            return;
        }
        res.end(result)
        res.writeHead(200,{
            'content-type':type
        })
    })

})
app.listen(3000,()=>{
console.log('success server');

})
```

### async的api
```
function getMsg(callback){
    setTimeout(()=>{
        callback({
            msg:'hello node js'
        })
    },2000)
}
getMsg(function(data){    //不会堵塞下面代码的执行，两秒中之后执行回调函数打印data
    console.log(data); //data就是函数中传给回掉函数的参数对象
})
```
想依次读取文件那么将下一次读取的文件放在上一次读取文件回调函数中
### promise

```
let promise = new Promise((resolve,reject)=>{
    fs.readFile('./1.txt','utf8',(err,result)=>{
        if(err!=null){
            reject(err)
        }else{
            resolve(result)
        }
    })
})
promise.then(result=>{
    console.log(result);
}).catch(error=>{
    console.log(err)
})
```

then链式，下一个then获取的promise就是上一个then返回的promise
解决回调地狱
```
function p1(){
    return new Promise((resolve,reject)=>{
        fs.readFile('./1.txt',(err,result)=>{
            resolve(result)
        })
    })
}
function p1(){
    return new Promise((resolve,reject)=>{
        fs.readFile('./2.txt',(err,result)=>{
            resolve(result)
        })
    })
}
//依次调用then来进行先异步读取1.txt后读取2.txt
p1().then(result=>{
    console.log('1.txt:'+result)
    return p2()
}).then(result=>{
    console.log('2.txt:'+result)
})
```


异步函数async关键字
async function fn(){}  //定义了异步函数
```
async function fn(){
        return 123;
}
console.log(fn());//返回promise对象包裹的返回值
fn().then((value=>{
    console.log(value);
    throw '发生错误';
})).catch(err=>{   //捕捉到throw出来的错误
    console.log(err);
    
})
```

await关键字,await会将返回值拿出来，只能在异步函数中执行
await后面跟promise函数，之后执行promise之后才会进行下一步执行
```
async function p1(){
    return 'p1'
}
async function p2(){
    return 'p2'
}
async function p(){
    console.log(await p1());
    console.log(await p2());
    
}
p()
```

### promiisfy
在util模块中，改造现有的异步函数，是其能返回promise对象
```
const fs = require('fs')
const promisify = require('util').promisify
const readFile = promisify(fs.readFile)
async function run (){
    let r1 = await readFile('./1.txt','utf8')
    let r2 = await readFile('./2.txt','utf8')
    console.log(r1);
    console.log(r2);
}
run()
```

### global
在node中global是全局对象
里也有console等方法
