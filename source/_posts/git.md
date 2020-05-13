---
title: git
date: 2020-05-08 17:56:48
tags:
    - git
categroies: GIT版本控制系统
---
## git命令
git config --global user.name xxx
git config --global user.email xxx
git config --list    查看git配置
git init 初始化
git status 查看文件状态 查看哪些没有被git管理
git add 文件列表 追踪文件
git commit -m 提交信息 向git仓库提交代码
git log 查看提交记录
git checkout 文件 用暂存区中的文件覆盖工作目录中的文件
```
使用场景：当你将文件提交到暂存区，之后继续在工作区里工作但是你发现修改的不好，想恢复之前上传缓存区的版本，用 git checkout 文件名 就可以恢复了
git checkout list.txt    //将暂存区中的list.txt文件覆盖工作区的文件，
```
git rm --cached 文件名 将文件从暂存区中删除
git reset --hard commitID 从git仓库中指定的更新记录恢复过来
### git 分支
当第一次提交的时候会自动产生一个分支master
基于master分支创建开发分支
基于开发分支创建功能分支，之后将功能分支合并到开发分支上 master是主分支用来发布
*分支命令*
git branch 查看分支
git branch 分支名称   创建分支
git checkout 切换分支
git merge  来源分支 合并分支
git branch -d 分支名称   删除分支 （分支没合并不能删除但是可以强制删除）（-D强制删除 ）

git 的暂时保存暂存区
git stash 存储临时改动
git stash pop 恢复改动
### 远程仓库
git push 推送到远程仓库 
git remote add origin github的url。给远程仓库起别名为origin
git push -u origin master。-u 会记住分支和仓库名称
克隆仓库
git clone 仓库地址
github中的setting中collaborators（合作者）来添加合作者
git pull 拉去更新
提交冲突
git push 失败
用git pull 拉取最新的 手动解决冲突后在提交
ssy-keygen生成ssh密钥
git add . 添加所有文件到暂存区
