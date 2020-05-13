---
title: 正则
date: 2020-04-01 18:46:18
tags:
     -js
categories: 前端
---
## 模块化
import {} from '路径'//倒入模块中的什么功能 script标签要写type = "module"
export {}  //导出模块化的什么功能
模块后解析且默认严格模式
模块有独立作用域
模块只解析一次
具名导出 export let site="zzx" 
具名导出可以导出函数类变量等等但是不能匿名
export{名1，名2，名3...}
批量导出倒入
```
import * as api from "path"
//用api.名来待哦用
```
具名导出的 时候倒入需要同名倒入

别名
```
导出的时候
export {site as hd}  //导出时把site别名为hd
import{ site as hd}  //倒入时别名hd
```

default默认导出,默认导出只有一个
```
export default class User{}  //1默认导出，没类名也没事
export {site as default}  //2默认导出
import  user from "path" //随便起一个变量就能接住默认导出,接住默认导出的名字不要瞎起
```

混合导入导出
```
import hd,{site} from "path"  //没有{}的就是默认导出有的就是普通导出
export{ User as default ,site}  //很容易看出哪个是默认导出的普通导出
import * as api form "path"  //全部导出时默认的怎么使用
api.default用
```
import("path").then(function(){})  //按需加载用这个


## 正则表达式

控制字符串的格式增删改查
创建
```
let hd = "zzx"
/u/.text(hd)  //检测字符串中是否有u
```

正则里面//包裹不能是变量，
```
let hd = "houdunren.com";
let a = "u";
console.log(/a/.test(hd)); //false
```

虽然可以使用 eval 转换为js语法来实现将变量解析到正则中，但是比较麻烦，所以有变量时建议使用下面的对象创建方式,eval将js语法转换成正则表达式用法如下
```
let hd = "houdunren.com";
let a = "u";
console.log(eval(`/${a}/`).test(hd)); //true
```

对象创建正则
```
let reg = new RegExp("u","g")  //对象创建正则表达式，检测u在全局里,第一个参数可以是变量
```

正则用法
```
/aaaa|b/  aaaa或者b
原子表原子组（）[]
/[123456]/    1 or 2 or 3 or 4 ... 原子表中有啥就匹配什么
[^...]     原子表中的^代表除了
()        原子组，一个整体
+         代表一个或者多个，\d+ 代表一个数字或者多个数字，只影响+之前的一个字符
*         代表0个或者多个
{a,b}   a个到b个 不写b就是a个无上限
/(1|2)34}/    134 or 234 ,()里是整体
.            除换行外任何字符
d            普通d
？            ？前可有可无0 or 1
在正则中直接空格也能算字符
转义
\.          普通.
\d          数字
\D          除了数字
\s          空白
\S          除了空白
\w          字母数字下划线
\W          除了字母数字下划线
通过对象创建的正则和字面量的正则不一样
/\d+\.\d/   =>   new RegExp("d+.d")
\1         第一个原子组
^         开头边界
$         结束边界
/[a-z]{3,6}/     字母3-6位
/^[a-z]{3-6}$/   整体是3-6位字符串
模式
/\d/g        匹配所有数字,g就是全局匹配
/[a-z]/i     不区分大小写
m模式单独对待每一行视为多行
u正确处理四个字符的 UTF-16 编码
//每个字符都有属性，如L属性表示是字母，P 表示标点符号，需要结合 u 模式才有效。其他属性简写可以访问 属性的别名 网站查看。
y从 regexp.lastIndex 开始匹配
/\.+/s     匹配所有 将换行当成普通空白，本来.不能匹配换行但是s模式可以让他匹配，将换行当作普通空白
/[\s\S]+/   匹配所有字符同理[\d\D]
模式可以混合使用直接写就行比如gi   ...
```


属性
```
//使用\p{L}属性匹配字母
let hd = "houdunren2010.不断发布教程，加油！";
console.log(hd.match(/\p{L}+/u));

//使用\p{P}属性匹配标点
console.log(hd.match(/\p{P}+/gu));
字符也有unicode文字系统属性 Script=文字系统，下面是使用 \p{sc=Han} 获取中文字符 han为中文系统，其他语言请查看 文字语言表
let hd = `
张三:010-99999999,李四:020-88888888`;
let res = hd.match(/\p{sc=Han}+/gu);  //搭配u使用
console.log(res);
RegExp对象lastIndex 属性可以返回或者设置正则表达式开始匹配的位置
必须结合 g 修饰符使用
对 exec 方法有效
开始lastIndex是0
匹配完成时，lastIndex 会被重置为0
reg.exec(str)  //检索字符串并且搭配g使用，改变lastIndex 每次循环能检索每一个匹配项且还有他们的属性
let hd = `后盾人不断分享视频教程，后盾人网址是 houdunren.com`;
let reg = /后盾人(.{2})/g;
reg.lastIndex = 10; //从索引10开始搜索
console.log(reg.exec(hd));
console.log(reg.lastIndex);

reg = /\p{sc=Han}/gu;
while ((res = reg.exec(hd))) {  //exec会记录上次检索到的位置下一次从检索位置开始检索
  console.log(res[0]);
}
```

y模式
从lastIndex开始检索检索不成功就不继续匹配了

关于子面量和对象创建正则
```
let price = 12.23;
//含义1: . 除换行外任何字符 	含义2: .普通点
//含义1: d 字母d   					含义2: \d 数字 0~9
console.log(/\d+\.\d+/.test(price));

//字符串中 \d 与 d 是一样的，所以在 new RegExp 时\d 即为 d
console.log("\d" == "d");

//使用对象定义正则时，可以先把字符串打印一样，结果是字面量一样的定义就对了
console.log("\\d+\\.\\d+");
let reg = new RegExp("\\d+\\.\\d+");
console.log(reg.test(price));

```
下面是网址检测中转义符使用
```
let url = "https://www.houdunren.com";
console.log(/https?:\/\/\w+\.\w+\.\w+/.test(url));
//?代表有没有
```

原子表
```
[]	只匹配其中的一个原子
[^]	只匹配"除了"其中字符的任意一个原子
[0-9]	匹配0-9任何一个数字,可以任意区间，升序
[a-z]	匹配小写a-z任何一个字母
[A-Z]	匹配大写A-Z任何一个字母
[()]    ()就是普通的括号
[a-z0-9]  a-z or 0-9
```
原子组
```
在match中使用原子组匹配，会将每个组数据返回到结果中
引用分组
\n 在匹配时引用原子组， $n 指在替换时使用匹配的组数据。下面将标签替换为p标签
let hd = `
  <h1>houdunren</h1>
  <span>后盾人</span>
  <h2>hdcms</h2>
`;

let reg = /<(h[1-6])>([\s\S]*)<\/\1>/gi;
console.log(hd.replace(reg, `<p>$2</p>`));
//replace(reg,(p0,p1,p2...))  //p0是匹配到的字符串，p1以后是原子组
(?:)   在匹配时不显示这个分组 不记录
组别名使用 ?<> 形式定义，下面将标签替换为p标签
let reg = /<(?<tag>h[1-6])>(?<con>[\s\S]*)<\/\1>/gi;
console.log(hd.replace(reg, `<p>$<con></p>`));
别名的原子组存在groups属性中通过.groups[name] 取出
```

正则方法
str.search(reg)  
str.replace(reg,(v,p1,p2)=>{}),  v匹配内容，p是原子组  函数返回新数组
str.match(reg) 返回搜索数组
str.matchAll(reg) 返回迭代对象
str.split(/[-/]/)  按照正则的规则拆分如例子按照-or/拆分
reg.test(str)  
reg.exec(str) 搭配g使用循环可得到有细节的匹配内容

$&就是匹配内容 $`匹配内容左侧 $'匹配内容右侧
替换字符串可以插入下面的特殊变量名：
变量	说明
$$	插入一个 "$"。
$&	插入匹配的子串。
$`	插入当前匹配的子串左边的内容。
$'	插入当前匹配的子串右边的内容。
$n	假如第一个参数是 RegExp 对象，并且 n 是个小于100的非负整数，那么插入第 n 个括号匹配的字符串。提示：索引是从1开始
多个正则应用到一个里面
```
let input = document.querySelector(`[name="password"]`);
input.addEventListener("keyup", e => {
  const value = e.target.value.trim();
  const regs = [/^[a-zA-Z0-9]{5,10}$/, /[A-Z]/];
  let state = regs.every(v => v.test(value));
  console.log(state ? "正确！" : "密码必须包含大写字母并在5~10位之间");
```

禁止贪婪
*?	重复任意次，但尽可能少重复
+?	重复1次或更多次，但尽可能少重复
??	重复0次或1次，但尽可能少重复
{n,m}?	重复n到m次，但尽可能少重复
{n,}?	重复n次以上，但尽可能少重复 

match 全局获取页面中标签内容，但并不会返回匹配细节
matchAll
在新浏览器中支持使用 matchAll 操作，并返回迭代对象
```
let str = "houdunren";
let reg = /[a-z]/ig;
for (const iterator of str.matchAll(reg)) {
  console.log(iterator);
}
```

自己写matchAll()
```
String.prototype.matchAll = function(reg){
    let res = this.match(reg);
    if(res){
        let str = this.replace(res[0],"^".repeat(res[0].length))
        let match = str.matchAll(reg) || [];
        return [res,...match]
    }
}
```

exec配合g才能全局匹配记录上次开始匹配的索引

断言匹配(匹配条件)
零宽先行断言(?=exp)匹配后面为 exp 的内容
零宽后行断言(?<=exp)匹配前面为 exp 的内容
零宽负项先行断言(?!exp) 零宽负向先行断言 后面不能出现 exp 指定的内容
零宽负向后行断言(?<!exp)前面不能出现exp指定的内容
