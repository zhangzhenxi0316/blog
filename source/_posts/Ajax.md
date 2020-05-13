---
title: Ajax与模版引擎
date: 2020-04-30 10:11:08
tags:
    - js
categories: 前端
---

实现局部更新数据，无刷新
运行在网站环境才能生效，会用到node
ajax相当于浏览器发送请求与接受响应的代理人
步骤
创建ajax对象
```
let xhr = new XMLHttpRequest();
```
告诉ajax请求地址和方式
```
xhr.open('get','http://www.baidu.com')
```
发送请求
```
xhr.send()
```
服务器端对客户端的响应
因为发送请求并且返回是有时间的所以响应回来的数据需要用时间onload事件触发接收responseText
```
xhr.onload = function(){
console.log(xhr.responseText)
}
```
服务端一般会响应json对象作为响应的格式
```
res.send({name:'zzx'})   //send可以返回json对象，但是会转换成字符串类型
客户端：
JSON.parse(xhr.responseText)//将json字符串转换成json对象
```
ajax传递参数
```
xhr.open('get','http://www.baidu.com?name=zzx&age=20')
```
ajax的post请求
```
xhr.open('post','http://www.baidu.com')
xhr.setRequestHeader('Content-Type','application/xxx-www-form-urlencoded')
xhr.send('name=zzx&age=50')

服务端
const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded())
```
post请求不仅可以在send里发送键值对形式，也可以发送json字符串
```
xhr.open('post','http://www.baidu.com')
xhr.setRequestHeader('Content-Type','application/json)
xhr.send(JSON.stringify({name:'zzx',age:30})) 

服务端
const bodyParser = require('body-parser')
app.use(bodyParser.json())
```
xhr.readystate状态码
0 代表还未创建xhr对象
1 代表创建了但是未发送
2 代表发送
3 代表接受到部分数据了
4 代表全部接受完毕服务端的数据
onreadstatechange事件可以监听状态码的改变接收服务器端传来的数据
推荐onload的事件
用onload事件接受不兼容低版本的浏览器，不需要判断状态码的变化，而onreadystatechange事件会多次被调用所以需要对状态码进行判断在获取数据
```
xhr.onreadystatechange = function(){
if(xhr.readyState == 4){
let text = xhr.responseText
}
}
```
ajax错误处理
```
1.网络通畅服务器端能接收到请求，服务器端返回的结果不是预期请求
xhr.status获取http状态码
2.网络畅通，服务器没有接受到请求，返回404请求，请求地址不存在
3.网络畅通服务器端能接受请求，服务器端出错返回500状态码
3.网络中断请求无法发送到服务器端
会触发xhr对象下面的error事件，在onerror事件处理函数对错误进行错误处理，无法触发onload事件
```
低版本IE的ajax缓存问题
解决：保证请求地址的后面加请求参数，保证每一次的请求参数值不同
```
xhr.open('get','http://www.baidu.com?t='+Math.random())
```
ajax封装

xhr.getResponseHeader获取相应头中的数据
```
let contentType = xhr.getResponseHeader('Content-Type')
contentType.includes()来判断是否包含json，html等信息
```
封装的时候用传入的对象覆盖默认的对象
```
Object.assign(defaults,options)
```
最终封装的结果
```
function ajax(options){
    let defaults = {
    url:'',
    type:'get',
    data:{},
    header:{'Content-Type':'application/x-www-form-urlencoded'},
    success:function(){},
    error:function(){}
    };
    Object.assign(defaults,options)//用传入的覆盖默认的
    let xhr = new XMLHttpRequest()
    let params = '';
    for(let i in defaults.data){
    params += `${i}=${defaults.data[i]}&`
    }
    params = params.substr(0,params.length-1);
    if(defaults.type == 'get'){
    defaults.url = defaults.url + '?' + params;
    };
    xhr.open(defaults.type,defaults.url);
    if(defaults.type == 'post'){
    let contentType = defaults.header['Content-Type'];
    xhr.setRequestHeader('Content-Type',contentType);
    if(contentType == 'application/json'){
        xhr.send(JSON.stringify(defaults.data))
    }else{
        xhr.send(params)
        }
    }
    else{
        res.send()
    }
    xhr.onload = function(){
    let contentType = xhr.getResponseHeader('Content-Type')
    let responseText = xhr.responseText
    if(contentType.includes('application/json')){
    responseText = JSON.parse(responseText)
        }
        if(xhr.status == 200){
        defaults.success(responseText,xhr)
        }else{
        defaults.error(responseText,xhr)
        }
    }
}
```
模版引擎art-template
使用步骤
```
1.安装 下载并在script标签中引用
<script src="./js/template-web.js"></script>
2.准备art-template模版，用id区分模版
<script id="tp1" type="text/html"></script>  //将script的type指定为html就会将里面的代码按照html解析
3.告诉模版引擎将哪个模版和哪个数据进行拼接
let html = template('tp1',{username:'zzx'})  //返回拼接好的字符串
4.将拼接好的html字符串添加到页面中
document.getElementById('container').innerHTML = html;
5.通过模版语法拼接html字符串和数据
```
文本框输入事件oninput事
做搜索框提示信息案例的时候oninput会在你键盘输入拼音的时候也触发，那么将会发送很多不必要的请求，所以需要用定时器来控制发送
## FormData对象
1. 模拟html表单 相当于将html表单映射成表单对象自动将表单对象中的数据拼接成请求参数的格式
2. 异步上传为紧致文件 图片视频等
使用
```
1.准备html表单 表单项需要name属性，不用写action
2.将html表单转化成FormData对象
let form = document.getElementById('form')
let formData = new FormData(form)
3.提交表单对象，只用与post请求
xhr.send(formData)
服务器端还是要用formidable去解析req
```
FormData对象实例方法
获取表单对象中属性的值
```
formData.get('username') 
```
设置表单对象中属性的值,如果没有则设置的表单对象属性
```
formData.set('username','zzx')
```
删除表单对象中的属性的值
```
formData.delete('username')
```
在表单对象中追加属性值,append会保留同名的属性
```
formData.append('username','zzx')
```
formData二进制文件上传
```
//获取文件选择控件
let file = document.getElementById('file')
file.onchange = function(){
let formData = new FormData();
formData.append('attrName',this.files[0])
}
xh.send(formData)
在服务器端
app.post('/upload',(req,res)=>{
    let form = new formidable.IncomingForm();
    form.uploadDir = path.join(__dirname,'public/upload')
    form.keepExtensions = true
    form.parse(req,(err,fields,files)=>{
        res.send(files)
    })
})
```
formData文件上传进度展示
在xhr的upload属性下有个onprogress事件在文件上传的过程中会持续触发这个事件
```
file.onchange = funciton(){
    //文件上传过程中持续触发onprogress事件
    xhr.upload.onprogress = function(ev){
    //当前上传文件大小/文件总大小在将结果转换成百分数
    //将结果赋值给进度条的宽度属性
    //ev.loaded是文件已经上传了多少
    //ev.total是文件的总大小
    bar.style.width = (ev.loaded/ev.total)*100 + '%';
}
}
```
formData文件上传图片即使预览
在我们将图片上传到服务器端以后，服务器端通常会将图片地址作为响应数据传递到客户端，客户端可以从响应数据中获取地址，之后显示在页面中

## Ajax的请求限制
JSONP解决同源限制问题
jsonp是json with padding 的缩写不属于ajax的请求，但可以模拟
script标签src可以写不同源的地址
服务器端响应的数据必须是一个函数的调用，真正要发送给客户端的数据需要作为函数调用的参数
```
const data = 'fn({name:'zzx',age:30})'
res.send(data)
```
在客户端要进行函数的定义
```
function fn(data){}
```
服务器端
```
res.jsonp({name:"zzx"})
```

## 加上jsonp之后的优化代码ajax
```
function ajax(options) {
    let defaults = {
        url: '',
        type: 'get',
        data: {},
        header: { 'Content-Type': 'application/x-www-form-urlencoded' },
        success: function () { },
        error: function () { }
    };
    Object.assign(defaults, options)//用传入的覆盖默认的
    let xhr = new XMLHttpRequest()
    let params = '';
    for (let i in defaults.data) {
        params += `${i}=${defaults.data[i]}&`
    }
    params = params.substr(0, params.length - 1);
    if (defaults.type == 'jsonp') {
        params=''
        for (let i in defaults.data) {
            params += `&${i}=${defaults.data[i]}`
        }
        let script = document.createElement('script');
        let fnName = 'myjsonp' + Math.random().toString().replace('.','')
        defaults.url = defaults.url + '?callback=' +fnName + params;
        window[fnName] = defaults.success
        script.src = defaults.url;
        document.body.appendChild(script)
        script.onload = function () {
            document.body.removeChild(script)
        }
    } else {
        if (defaults.type == 'get') {
            defaults.url = defaults.url + '?' + params;
        };
        xhr.open(defaults.type, defaults.url);
        if (defaults.type == 'post') {
            let contentType = defaults.header['Content-Type'];
            xhr.setRequestHeader('Content-Type', contentType);
            if (contentType == 'application/json') {
                xhr.send(JSON.stringify(defaults.data))
            } else {
                xhr.send(params)
            }
        }
        else {
            xhr.send()
        }
        xhr.onload = function () {
            let contentType = xhr.getResponseHeader('Content-Type')
            let responseText = xhr.responseText
            if (contentType.includes('application/json')) {
                responseText = JSON.parse(responseText)
            }
            if (xhr.status == 200) {
                defaults.success(responseText, xhr)
            } else {
                defaults.error(responseText, xhr)
            }
        }
    }
}
```

向template模版里面传函数
```
template.defaults.imports.dateFormat = dateFormat
```
## CORS
Cross-origin resource sharing跨域资源请求
当发起跨域请求ajax时浏览器会发送个请求头里有origin属性，保存的是请求的地址，服务器端同意请求的话会在响应头中写Access-Control-Allow-Origin字段
```
相当于白名单 * 代表都可以跨于访问
Access-Control-Allow-Origin:'http://baidu.com
Access-Control-Allow-Origin':'*'
```
服务器端,设置跨域访问
```
const express=require('express')
const app  = express()
app.get('/cros',(req,res)=>{
    //允许所有的客户端访问我
    res.header('Access-Control-Allow-Origin','*')
    //允许客户端使用那些方法访问我
    res.header('Access-Control-Allow-Method','get,post')
    res.send('ok')
})
app.listen(3030)
```
中间件来设定,拦截所有的请求之后进行设置
```
app.use((req,res,next)=>{
//允许所有的客户端访问我
res.header('Access-Control-Allow-Origin','*')
//允许客户端使用那些方法访问我
res.header('Access-Control-Allow-Method','get,post')
next()
})
```
### 另一种非同源访问的方法
用自己的服务器来访问非同源的服务器，之后用自己的服务器所返回来的数据，简单的来说用自己的服务器导个手，因为服务器端是没有什么同源限制的,用自己的服务器来访问别的服务器需要用到第三方模块request
```
const request = require('request')
app.get('/cros',(req,res)=>{
    request('http://localhost:3030/cros',(err,response,body)=>{  //此服务器在80端口用这个服务器跨于访问3030端口之后用本服务器返回给客户端数据
        // console.log(err);
        // console.log(response);
        // console.log(body);
        res.send(body)
    })
})
```
ajax下跨域请求不会发送cookie
withCredentials属性 指定在涉及到跨域请求的时候是否携带cookie默认为false
```
xhr.withCredentials = true
```
服务器还要写这个字段
```
res.header('Access-Control-Allow-Credentials','true')
```
## jq中的ajax
对象属性
type  data url contentType success error 
beforSend：(XHR)=>{}请求发送之前调用的函数
data可以写成对象形式也可以写成字符串之间用'&'分隔
dataType:'jsonp'，预期服务器返回的数据类型。如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断，比如XML MIME类型就被识别为XML。
jsonp属性修改callback参数名称，在一个jsonp请求中重写回调函数的名字。这个值用来替代在"callback=?"这种GET或POST请求中URL参数里的"callback"部分，比如{jsonp:'onJsonPLoad'}会导致将"onJsonPLoad=?"传给服务器。
jsonpCallback指定函数名称为jsonp请求指定一个回调函数名。这个值将用来取代jQuery自动生成的随机函数名。这主要用来让jQuery生成度独特的函数名，这样管理请求更容易，也能方便地提供回调函数和错误处理。你也可以在想让浏览器缓存GET请求的时候，指定这个回调函数名。
$.get(url,{},function(response){})
$.post(url,{},function(response){})
### jq中的serialize方法可以拼接表单参数
### jq中的ajax全局方法
绑定在document上，当页面有ajax请求时触发
.ajaxStart().当请求开始发送的时候触发
.ajaxComplete()当请求完成的时候触发
## NProgress
纳米级进度条，使用逼真的涓流动画来告诉用户正在发生的事
```
<link rel="stylesheet" href="nprogress.css">
<script src="nprogress.js"></script>
NProgress.start()//进度条开始运动
NProgress.done()//进度条结束运动
```
## RESTful风格的api
一套关于设计请求的规范
get 获取数据
post 添加数据
put 更新数据
delete 删除数据
在ajax中支持put和delete但是表单提交不支持

```
app.get('/users/:id',(req,res)=>{
    //id是占位符代表传过来的参数
    //req.params.id来获取
    const id = req.params.id
})
app.delete('/users/:id',(req,res)=>{})//delete请求，put同样
```
## XML
```
res.responseXML  //用来获取响应的xml
res.header('content-type','text/xml')
res.send('<message>哈哈哈</message>')
用.getElementsByTagName()[0].innerHTML可以获取标签内容
```
