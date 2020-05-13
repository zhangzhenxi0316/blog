---
title: jQuery学习笔记1
date: 2020-03-23 21:21:20
tags:
    - jQuery

categories: 前端
---

# 【javascript】jQuery快速学习

整理了一些简单的jQueryAPI，仅供参考具体详情参考官方文档

## jQ选择器

jq中用 **$** 来代表jQuery

语法：
$(selector)

jq筛选选择器常用：
:first  第一个 例子：$("div:first)之后的类似
:last 最后一个
:eq(index) 第index个
:odd 奇数
:even 偶数
:header 匹配标题元素h1等
:animate 正在执行动画的元素
:has()匹配含有选择器所匹配的元素的元素
:hidden 隐藏的
:visible 可见的
[attribute[!^$*='value']] 匹配指定属性的元素*非，开始，结尾，包含*
:first-child  第一个子元素
:only-child  如果某个元素是父元素中唯一的子元素，那将会被匹配
:(表单属性) 选择指定表单
:enabled  匹配所有可用元素
:disabled  匹配所有不可用元素
:checked  匹配所有选中的被选中元素(复选框、单选框等，select中的option)
:selected  匹配所有选中的option元素

<!-- more -->
## jQ方法

### 筛选函数

.parent()  查找父级
.parents()  祖先级元素
.children(selector)  查找子元素可指定选择器
.find(selector)  查找后代可指定选择器
.siblings(selector)  查找兄弟节点可指定选择器
.nextAll()  查找当前元素之后的所有元素
.prevAll()  查找当前元素之前的所有元素
.hasClass(className)  查找当前元素是否有某个特定类有返回true
.eq(index)  与:eq(index)一样，index从0开始

### 事件函数

**可以传参数data 和function，data是传入参数供函数使用的可以不写**
.mouseover() 鼠标经过事件
.mouseout() 鼠标离开
.mousedown() 鼠标按下
.mouseup()  鼠标起来
.mouseenter() 鼠标进入 进入子元素不会再次触发而over会
.mouseleave()  鼠标离开 与enter性质差不多
.mousemove()  鼠标移动
.resize()  浏览器窗口调整大小事件
.scroll()  指定元素滚动事件
.select() 当 textarea 或文本类型的 input 元素中的文本被选择时，会发生 select 事件
.submit()  提交表单事件
.blur()  失去焦点事件
.focus()  获取焦点事件
.focusin()  获取焦点事件 *和focus不同的是，可以在父素监测到子元素的focusin，也就是在父元素上绑定事件，当子元素获取焦点是也会触发*
.focusout()失去焦点
.change() 改变事件 *一般用在表单元素*
.click() 鼠标点击事件
.dbclick() 鼠标双击函数**尽量不要和click混合用**
.keydown()  键盘按下
.keypress() 键盘按下, [区别](https://blog.csdn.net/winsolstice/article/details/78842805)
.keyup()  键盘抬起
.ready(fn)  页面载入事件
.hover(function1, function2) 1为mouseover的函数，2为out函数
on(events, [selector], [data], fn)  绑定事件，可用对象形式传参，可以绑定多个事件，推荐使用
.off()解绑
.trigger() 自动触发事件 这个函数也会导致浏览器同名的默认行为的执行。比如，如果用trigger()触发一个'submit'，则同样会导致浏览器提交表单。如果要阻止这种默认行为，应返回false。

``` 
$("p").click( function (event, a, b) {
  // 一个普通的点击事件时，a和b是undefined类型
  // 如果用下面的语句触发，那么a指向"foo",而b指向"bar"
} ).trigger("click", ["foo", "bar"]);
```

.triggleHandler()  自动触发事件但是不会触发默认行为

#### 事件对象

e.stopPropagation() 组织冒泡行为
e.preventDefault() 阻止默认行为
e.pageX 鼠标位于文档左侧距离
e.pageY 鼠标位于文档上侧距离
... 

### 效果函数

**speed是速度，e用来指定切换效果，默认是"swing"，可用参数"linear" fn回调函数**
.show([speed, [e], [fn]]) 显示效果
.hide([speed, [e], [fn]])  隐藏效果
.toggle([speed, [e], [fn]])  隐藏和显示切换效果
.slideDown([speed, [e], [fn]])  向下滑动出现效果
.slideUp([speed, [e], [fn]]) 向上滑动消失效果
.slideToggle([speed, [e], [fn]]) 滑动切换效果
.fadeIn([speed, [e], [fn]])  淡入效果
.fadeOut([speed, [e], [fn]])  淡出效果
.fadeTo([speed, [e], [fn]]) 指定变化
.fadeToggle([speed, [e], [fn]]) 淡入淡出切换
.animate(p, [s], [e], [fn])  p 想要更改的多个属性以对象形式传
.stop()  用于上一次触发的动画效果防止多次触发，立马执行队列中所有动画
.delay() 延迟动画效果

### 设置获取属性

.attr()  设置或返回属性，参数可以是一个或者两个参数一个属性一个值，或者以对象形式发送
.removeAttr()  移除属性
.prop()  查看或修改固有属性，获取在匹配的元素集中的第一个元素的属性值。
.removeProp()移除
.addClass([class, [fn]])  添加类名, fn需要返回一个class类名
.removeClass([class, [fn]])  移除类名
.toggleClass()  切换类名
.html()  获取html值或者覆盖原有值
.text()  获取文本值或覆盖原有值
.val()  用于表单元素

### 数据缓存

.data(key, value)

``` 
$("div").data(key,value)在div上存了一个数据key:value
$("div").data("test", { first: 16, last: "pizza!" });
$("div").data("test").first  //16;
$("div").data("test").last  //pizza!;
```

.removeData()移除数据

### 对象访问

.each() 以每一个匹配的元素作为上下文来执行一个函数。
.length() 匹配到元素的长度(个数)
.get(index) 第index个元素
.index(selector) 返回元素在匹配的选择器中的位置

### 事件处理

$("\<li\>\</li\>")  创建标签，可拼接字符串 举一反三
.append(内容) 元素内部最后添加内容(节点)
.appendTo(指定地方)，将内容加在指定的地方内，颠倒了常规的append
.prepend(内容)  元素内部最前面添加内容(节点)
.prependTo(指定地方) 
.after(内容)  在指定元素后添加内容
.before(内容)  在指定元素添加内容
.remove()  移除指定元素(自杀)
.empty()  清空指定元素后代被清空
.clone()  克隆指定元素并返回

### css

.css(name, value, [对象]) 更改或者设置属性
.width()/.height()  返回匹配元素的width和height值可以修改
.innerWidth()/.innerHeight() 带padding的w与h
.outerWidth()/.outerHeight()  带border padding的w和h
.outerWidth(true)/.outerHeight(true)  带margin padding border的w和h
.offset()  距离文档位置返回对象  .offset().top, .offset().left, 传参数可以改变位置，对象形式传参
.position() 获得在带有定位父级的位置坐标，不能设置值
.scrollTop()/.scrollLeft()  元素被卷去的部分可设置值

### 字符串操作
.trim(str)去除前后空格

### 多库共存

$.noConflict()
用name代替$

``` 
var name = $.noConflict()
```



