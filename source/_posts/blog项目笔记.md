---
title: blog项目笔记
date: 2020-04-21 20:15:28
tags:
---
1.jq提供的方法获取表单的值
.serializeArray()  //返回数组
自己可以以这个方法为基础封装个函数来获取表单属性变成{name:'name',passward:''}
```
function serializeToJson(form) {
var result = {};
// [{name: 'email', value: '用户输入的内容'}]
var f =  form.serializeArray();  
f.forEach(function (item) {
    // result.email
    result[item.name] = item.value;
});
return result;
```
表单验证
1.在页面先进行1次验证
2.后台服务进行第二次验证

## 密码加密bcrypt模块
哈希算法加密
单程加密方式
bcrypt模块对环境依赖较高需要
python 2.x
node-gyp(全局安装)
windows-build-tools (npm install --global --production windows-build-tools)

使用
```
const salt = bcrypt.genSalt(10)//生成随机字符串默认复杂度为10，越大越复杂
const result = bcrypt.hash('12345',salt)//第一个参数是想加密的明文密码第二个是随机字符串
cont isvalue = bcrypt.compare('123456',result)//对比加密，返回bool值，真代表相同
```
## cookie与session
cookie：浏览器在电脑硬盘中开辟的一块空间，主要提供服务器端存储数据
cookie中的数据是以域名的形式进行区分的
cookie中的数据是有过期时间的超过时间的数据会被浏览器自动删除
cookie中的数据会随着请求自动发送到服务器端

session实际上就是一个对象，存储在服务器端的内存中，该对象中也可以存储数据每一条数据都有一个唯一的seasonId进行区分

nodejs使用express-session第三方模块使用session功能
```
const session = require('express-session')
app.use(session({secret:'secret key')

req.session.username= user.username  //session属性是express-session创建的
req.session.destroy(()=>{
res.redirect('')
res.clearCookie('connect.sid')
})
```

## express重定向
res.redirect('url')

## req
req.app.locals.userInfo = user  //在请求对象中有服务器对象这个属性
app.use('/admin',(req,res)=>{console.log(req.url)})  //这里面的url是/admin之后的访问的地址 

检查登陆功能用检查cookie和session
判断req.session是否存在

## joi第三方模块
js对象规则描述和验证器

```
const Joi = require('joi')
const Schema = {
username: Joi.string().alphanum().min(3).max(5).required().error(new Error('错误信息')),不能包含其他字符(alphanum)的字符串 error指定错误信息
password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/'),
access_token:[Joi.string(),Joi.number()],
birthyear:Joi.number().integer().min(1900).max(2013),
email:Join.string.email()
role:Joi.string().valid('normal','admin').error(new Error('角色值非法'))//只能传normal和admin
}
Joi.validate({name:'abc',birthyear:1994},schema)，验证
```

${{@$value._id}}//输出数据库数据的id要想不带引号输出 用@前缀输出原文

## 数据分页
User.countDocuments({})  //可以查到集合中有多少条数据
实现分页用到limit(2)限制 skip(1)跳过


## next
next()//参数只能传字符串类型
如何触发错误处理中间件
next()里面传字符串参数时会触发错误处理中间件，对象用JSON.stringify(obj)来转换


## 关联集合
使文章集合的作者和user集合的id进行关联
```
author:{
    type:mongoose.Schema.Types.ObjectId,
    ref:'User'
}
```
## 文件上传的表单
form 属性enctype指定表单的编码类型，默认application/x-www-form-urlencoded  => name=zzx&age=17
要是想上传文件编码格式要是二进制 multipart/form-data
body-parser只能解析普通的表单的body
解析表单用formidable第三方模块，即可以解析get又可以解析post还可以解析二进制的表单
```
//引入formidable
const formmidable = require('formidable')
//创建表单解析对象
const form = new formidable.IncomingForm()
//设置文件上传路径
form.uploadDir = "/my/dir"
//是否保留表单上传的文件的扩展名
form.keepExtensions = true;
//表单解析
form.parse(req,(err,fields,files)=>{
//fields存储普通请求参数。对象类型
//files 存储上传的文件信息
})
```

## 文件读取FileReader
```
js的内置对象FileReader对象
let reader = new FileReader();
//读取二进制文件比如图片，这个方法是异步函数不能通过返回值的方法获取，所以用onload事件打印读取结果
reader.readAsDataURL('文件');
reader.onload = function(){
console.log(reader.result)
}
```
*input:file*
选择文件，multiple可多选属性，其标签的属性files属性是个选择的文件列表 
*img src*
Img的src属性如果是FileReader对象读取的结果编码也可以直接显示图片
```
let file = document.querySelector('#file')
    let preview = document.querySelector('.preview')
    file.onchange=function(){
        let reader = new FileReader();
        reader.readAsDataURL(this.files[0])
        reader.onload = function(){
            preview.src = reader.result
        }
    }

```
## 关联查询
```
Article.find().populate('author')
将查询到的作者字段进行关联查询
```
## 数据分页 mongoose-sex-page第三方模块
```
const pagination = require('mongoose-sex-page')
//page 当前显示页
//size每一页多少数据
//display页面显示总页数
//exec发送数据库查找请求
pagination(集合构造函数).page(1).size(20).display(8).exec()
```
## mongoDB数据库添加账号
以管理员的方式运行power shell
链接数据库mongo
查看数据库 show dbs
切换到admin数据库 use admin
创建超级管理员账户 db.createUser()
切换到blog数据库 use blog
创建普通账号 db.createUser()
卸载mongodb服务
停止服务net stop mongodb   ，  mongod  —remove
```
//查看数据库
> show dbs
admin       0.000GB
blog        0.000GB
config      0.000GB
local       0.000GB
playground  0.000GB
//进入admin数据库
> use admin
switched to db admin
//user是用户名字，pwd密码 roles数组root代表超级管理员readWrite是不同读写用户
> db.createUser({user:'root',pwd:'root',roles:['root']})
Successfully added user: { "user" : "root", "roles" : [ "root" ] }
> use blog
switched to db blog
> db.createUser({user:'itcast',pwd:'itcast',roles:['readWrite']})
Successfully added user: { "user" : "itcast", "roles" : [ "readWrite" ] }
> exit
```
添加完账号在命令行中重新启动mongod
```
mongod --auth --dbpath=/Users/zzx/data
```
之后在nodejs中链接数据库时要加上用户名和密码中才能正确链接数据库
```
mongoose.connect('mongodb://itcast:itcast@localhost:27017/blog')
```
## 开发环境和生产环境
```
给电脑设置环境变量
vim ~/.zshrc
export NODE_EVN=development
```
在nodejs中通过global中的内置对象process对象下的evn查看当前的系统环境
```
console.log(process.evn.NODE_EVN)
```
## Morgan第三方模块
```
const morgan = require('morgan')
app.use(morgan('dev'))
之后客户端对服务器进行请求的时候会将信息打印在node控制台中，最好在开发环境中使用，
```
## config第三方模块
允许开发人员将不同运行环境下的应用配置信息抽离到单独的文件中，模块内部自动判断当前应用的运行环境，并读取对应的配置信息，极大提供应用配置信息的维护成本，避免了当运行环境多次切换时手动修改信息
使用步骤
1.安装
2.在项目的根目录下新建的config文件夹
3.在config文件夹下面新建default.json  development.json. production.json
4.引入
5.使用模块内部提供的get方法获取json中的配置信息
会自动判断当前的运行环境之后会读取不同的json文件
```
const config = require('config')
mongoose.connect(`mongodb://${config.get('db.user')}:${config.get('db.pwd')}@${config.get('db.host')}:${config.get('db.port')}/${config.get('db.name')}`, { useNewUrlParser: true, useUnifiedTopology: true }).then(() => { console.log('数据库链接成功') }).catch((err) => { console.log('数据库链接失败，原因：' + err) })

config.get获取的是对应的环境所匹配的json文件，比如开发环境get读取的是development.json
```

Config模块允许将敏感配置信息存储在环境变量中，
在config文件夹中建立custom-environment-variables.json文件，将敏感的配置放在这里面，敏感信息的值为填写系统变量的名字
