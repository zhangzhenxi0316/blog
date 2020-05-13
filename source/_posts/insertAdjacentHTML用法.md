---
title: insertAdjacentHTML用法
date: 2020-03-24 13:28:20
tags:
    --js
categories: 前端
---

# insertAdjacentHTML用法

## 语法
element.insertAdjacentHTML(position, text);
**position**
一个 DOMString，表示插入内容相对于元素的位置，并且必须是以下字符串之一：
'beforebegin'：元素自身的前面。
'afterbegin'：插入元素内部的第一个子节点之前。
'beforeend'：插入元素内部的最后一个子节点之后。
'afterend'：元素自身的后面。
***text**
是要被解析为HTML或XML元素，并插入到DOM树中的 DOMString。


appendChlid不支持追加字符串的子元素，只能通过createElement动态创建后在用innerHTML添加内容
insertAdjacentHTML支持追加字符串的元素
