---
title: html基础篇和css
date: 2020-04-22 09:05:44
tags:
---
# 献丑了

## html骨架
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```
## 简单介绍一下常用的标签
标签分为单标签和双标签
+ 单标签
  - \<br/>  换行
  -\<hr/>  分隔线
  - \<img>等。。后面有介绍
+ 双标签 
  + 文本格式标签
     - \<b>\</b> 加粗文本
     - \<strong>\</strong>加重语气(语义化标签)
     - \<em>\</em> 斜体
     - \<del>\</del>定义删除字
     - \<ins>\</ins>插入字
     - \<u>\</u>下划线
     - \<pre>\</pre>预文本
     - 等比较常用的，这里就不举例子了 [W3C](https://www.w3school.com.cn)有更多标签展示
  
  
### 网站常用的标签介绍
**链接标签**
\<a href="">\</a>   
href标签里面写跳转的地址a标签里面包含链接的内容
```
<a href="http://www.baidu.com">百度</a>
```

**图像链接**
\<img src="" alt=""  title="">\</img>
img的href属性中写地址，alt属性写图片的替代文本，title属性是鼠标移动到图片上显示的文本

```
<img src="./images/2.png" alt="替代的文本" title="鼠标移动上来现实的文本">
```

**表格**
\<table>\</table>
table标签中<tr></tr>标签代表行<td></td>标签代表列th代表表头，居中加粗显示
table属性：border可以设置单元格的边框，cellspacing设置单元格距离
td属性：colspan跨列合并，rowspan跨行合并

```
<table cellspacing="0"  border="1">
    <th>姓名</th>
    <th>年龄</th>
    <th>性别</th>
    <tr>
        <td>zzx</td>
        <td>18</td>
        <td rowspan="2">男</td>
    </tr>
    <tr>
        <td>tsy</td>
        <td>18</td>
    </tr>
</table>
```

**表单**
\<input>单标签
type指定表单类型，text文本输入框 password密码输入框 button 按钮 radio单选按钮(通过name属性来分组)checkbox(复选框)...等等表单的元素
一般表单元素是放在form表单中的
```
<form action="www.baidu.com"></form>
    <input type="text">
    <input type="" name="" id="">
    <input type="radio" name="radio" id="" >
    <input type="radio" name="radio" id="">
    <input type="checkbox" name="check1" id="">
    <input type="checkbox" name="check2" id="">
    <input type="submit" value="提交按钮">
    <input type="button" value="按钮">
</form>
```
h5新增了一些表单元素
+ color
+ date
+ datetime
+ datetime-local
+ email
+ month
+ number
+ range
+ search
+ tel
+ time
+ url
+ week
老版本的浏览器值当作text来显示
表单属性有value，readonly disabled maxlength size required placeholder等属性

**表单的下拉列表和预定义列表**
```
<select name="" id="">
    <option value="第一个">我是zzx</option>
    <option value="第二个">我是tsy</option>
</select>
```

```
<input list="browsers">
<datalist id="browsers">
    <option value="Internet Explorer">1
    <option value="Firefox">2
    <option value="Chrome">3
    <option value="Opera">4
    <option value="Safari">5
 </datalist> 
```

**列表**
\<ul>\</ul>无序列表
\<ol>\<ol>有序列表
\<li>\</li>列表项
dl  dt  dd自定义列表
```
<ul>
    <li>第一个</li>
    <li>第二个</li>
</ul>
<ol>
    <li>第一个</li>
    <li>第二个</li>
</ol>
<dl>
    <dt>头</dt>
    <dd>身体</dd>
</dl>
```


### 块元素
**div块元素**
这是一个常用的布局元素，布局html经常用div布局，之后用css去修饰他的样式达到效果
\<div>\</div> div包裹的内容是一个块，在页面中独占一行，也就是说

```
<div>第一</div><div>第二</div> //不显示在同一行
```
在这里说一下标签是有很多属性的建议大家去w3c看看入门教程，
比如所有标签都可以有id属性name属性class属性等
**span行内快**
\<span>\</span>标签是行内块，和div不同的是它可以在同一行来显示不会独占一行，也是经常用的布局手段
```
<span>电子</span><span>商务</span> //会显示在同一行
```

### 实体字符
常用的
+ \&nbsp;空格
+ \&lt;小于号
+ \&gt;大于号
+ \&amp;&号
+ ...有兴趣的查一下

## css
选择器
类选择器，针对class属性 .className{}
id选择器。 针对id属性。  #idName{}
标签选择器。    tagName{}
属性选择器。   [title=hello]{}  选则title属性是hello的元素
### 派生选择器
后代选择器
```
ul li{
color:black;
}
```

子代选择器
```
.father > .son{
border:1px solid black;
}
```
相邻兄弟选择器
```
h1 + h2{
margin:2px 3px 4px 5px;
}
```
选择器之间可以结合使用
```
html > body table + ul{
margin-top:20px;
```
选择器的分组
```
h1,h2,h3{
color:red;
}
```


