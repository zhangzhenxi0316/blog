---
title: mysql数据库学习
date: 2020-05-11 12:29:14
tags:   
    -mysql
categories: 数据库
---
mysql -uroot -p  登陆mysql需要输入密码 -P 3306  端口号  -h 127.0.0.1 链接的主机
链接本地用默认值就能链接
语句以分号结尾才会执行

```
//展示数据库
show databases;
```
如果想放弃这条语句
```
show databases \c  //放弃这条语句
```
exit退出

## 命令行语句
```
create database shop charset utf8;   //创建数据库shop 
use shop;  //use 切换数据库
drop database shop;  //删除数据库
drop database if exists shop//删除数据库如果存在的话。 最好在删除数据库的时候进行一次判读
show create database shop //查看创建的数据库的结构
```
navicat连接数据库报2059错误解决方法
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #修改加密规则 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456'; #更新用户密码 
FLUSH PRIVILEGES; #刷新权限 
```
外部sql文件操作方法
```
mysql -uroot -p < /Users/zzx/js/sql.sql  //执行外部文件sql.sql 可以写绝对路径
source /Users/zzx/js/sql.sql  //内部导入文件
```

建表和删除表格
```
//建立表格并制定列名和规则
create table class (id int primary key AUTO_INCREMENT,cname VARCHAR(30),description VARCHAR(30) default null) CHARSET UTF8;//描述默认是空
//降序表
desc class
drop table if EXISTS class;
//not null 就是不允许为空值
create table class (id int primary key  AUTO_INCREMENT,cname VARCHAR(30) not null ,description VARCHAR(30)) CHARSET UTF8;
desc class;
//添加数据的两种方式
insert into class set cname='php', description = 'php学习';
insert into class (cname,description) value('linux','服务器知识') ,('musql','数据库');
create table test like class ;  //基于class建立表格
//插入语句
INSERT INTO test select * from class;
INSERT INTO test (cname) select cname from class;
INSERT INTO test (cname,description) value('js','前端语言');
//创建表 并且导入数据
create table a (id int primary key auto_increment,cname varchar(30)) select cname from class;
//上面的如果创建表中没有cname那么会加一列名，现在下面的as会起别名，将cname加在name上
create table a (id int primary key auto_increment,name varchar(30)) select cname as name from class;
```

查询
```
select * from class;  //可以选择查询项
select cname from class where id>2 and cname='php';
//like 查询包含 关键字的值 % 是通配符 
select cname from class where description like '%p%';
//结合函数
select concat(cname,description) as class_info from class;//concat 是连接函数，链接字符串，cname和description的字符串
```

查询条件
```
//or代表或者 and 代表并且
select * from class where id > 2 OR cname = 'php';
//distinct 去重
select distinct cname from class where id > 2 ;
//between and 和 not between and 
select distinct id from class where between 2 and 7;
//in 和 not in
select * from stu class_id in(2,3); //查询id在2，3集合之间的
//null不能作为比较条件,null和自身比较都是false
select * from class where cname=null;//比较不出任何结果，null不能比较
//检查是否为空用is null 为不空 not null
select * from class where class_id is null//这么做就可以
//if函数,如果第一个参数的值是真则取第二个参数的值，如果未假就第三个参数的值
select sname,if(class_id ,class_id ,'未分配') from stu;
//ifnull函数 
select sname,ifnull(class_id ,'未分配') from stu;
```

排序 和数据区间筛选
```
//order by   ,desc 从大到小降序 asc从小到大 升序
select * from stu order by class_id asc ,age asc;  //先按照id取升序排列相等的话就用age来升序比较
//limit n 取n个数据
select * from stu order by class_id asc limit 2;//数据只取前两个
limit n ,m 从n开始取，取m个
select * from stu order by class_id asc limit 2,1; //从2开始只取一个
```

更新 删除数据
```
//set 后面可以加表达式
UPDATE class set description ='没有描述' where description is null;
//delete
delete from stu where age<30 order by id desc limit 2;
```
修改
```
//alter修改关键字
alter table stu rename stus;//重命名
rename table stus to stu;//重命名
alter table stu modify sname varchar(40);//修改表中sname的字段类型
alter table stu change sname name char(30) not null  //修改表中的sname字段重新命名为name
alter table stu add sex smallint default null after id;//添加字段在id后。first则是放在第一项
//delete是删除，一条一条删除数据
//truncate 清空表
truncate stu ;//清空表
//drop删除表
drop table if exists stu;

```
>>> change 改变表的列alter table 表名 change   列名 列名 属性 ;
>>> modify 改变表的列属性 alter table 表名 modify 列名 属性 ;
移除主键
```
alter table stu drop primary key
alter table stu add primary key (id)
```

## 数据类型
```
//查看系统支持的字符集
show character set
```
校对规则
默认字符集utf8_general_ci 就是查询的时候不区分大小写 后缀为bin则区分大小写
+ 常用的字符串处理函数
    - left(str,len)选择是取每一个字段的左面两个字如 left(cname,2) 原来的cname="zzx" 查询出来只有'zz'
    - right(str,len)
    - mid(str,len)从任意len开始取之后的所有
    - concat(a,b)连接函数，连接字符串
    - substring(str,len)和mid差不多，从第2个位置开始
    - char_length(str)算出长度
    - if(条件,true事件,false事件)
    - REPLACE(s,s1,s2)    将字符串 s2 替代字符串 s 中的字符串 s1
正则表达式
```
REGEXP '表达式'
```
like匹配
```
% 通配符
_ 代表一个字符
```
>>>  函数混用：查询的字段超过两位用省略号显示 select IF(CHARACTER_LENGTH(cname)>2,concat(left(cname,3),'...'),cname) as cname from class;
定义数据类型
unsigned 非负数 用于数值型
```
alert table class add status int(5)  //数值默认长度是5 当你输入10的时候00010 但是不会显示出来
alert table class add status int(5) zerofill //数值的默认长度是5 当小于5位的时候会在前面显示0 
```
+ 浮点数
    - float
    - double
    - decimal  decimal(总位数，小数位数)

enum类型（单选）
```
alter table class modify sex ENUM（'男','女');
insert into class (sex) vaules(1) //第一个值也就是男 
select * from class where sex =2//查找女的
```
set类型（多选）
```
alter table class add flag set('推荐','制定','热门');
//查找
select * from class where find_in_set('热门',flag)
```
二进制模糊匹配set
第一个值上面的二进制表示 推荐 0001 制定 0010 热门 0100  转为对应的十进制之后
```
select * from class where flag & 3 //查找包含制定或者推荐
```

​	





