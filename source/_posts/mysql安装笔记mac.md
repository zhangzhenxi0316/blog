---
title: mysql安装笔记mac
date: 2020-04-22 21:06:22
tags:
---
## mac安装sql
1.官网安装社区版的
2.新版的安装会有安装进程中设置密码，并结束后可能会自动开启服务
3.用sudo mysql.server start 来开启注意重复开启会报错会报出什么为更新pid的文件
4.mysql -u root -p 启动服务
5.set password = password('')设置密码
6.exit退出sql
6.停止MySQL服务sudo mysql.server stop
7.重启MySQL服务sudo mysql.server restart
8.查看MySQL服务状态sudo mysql.server status
