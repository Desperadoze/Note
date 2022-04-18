# Git

## Git安装

官网下载、逐步安装，安装完成后进行初始化，设置username 、useremail

![image-20220417121058155](C:\Users\Administrator.DESKTOP-5DOACST\AppData\Roaming\Typora\typora-user-images\image-20220417121058155.png)

## Git常用命令

#### git init

初始化，新建一个project后，在它的目录右键，git bash here。然后运行git init初始化

#### git status

查看本地库状态

#### git add

添加到暂存区

#### git commit -m “日志信息” 文件名

提交到本地库

#### 查看历史信息

**git reflog **查看日志信息

**git log **查看详细日志信息

#### git reset --hard 版本号

版本穿梭（指针移动，没有对文件进行操作）

## 分支

使用分支可以把自己的工作从开发主线上分离开来。可以理解为副本

#### git branch -v

查看分支

#### git branch 分支名

创建分支

#### git checkout 分支名

切换分支

#### git merge 分支名

正常合并

在当前分支下，合并命令中的分支

冲突合并：手动修改后再次使用git commit提交，**不能带文件名**

==HEAD 指针指向的是分支==

==master指针指向的是版本本身==

## Git 团队协作

#### 团队内协作：push	pull	clone

#### 跨团队协作：fork	clone	pull request	merge



## GitHub操作

#### git remote -v

查看当前别名

#### git remote add 库名 链接

创建远程库、创建别名

远程库的名称最好与本地库相同

#### git push 别名(链接) 分支名

推送本地库到远程库，最小推送单位是分支

#### git pull 别名(链接) 分支名

推送远程库到本地库，最小拉取单位是分支

#### git clone 链接

clone会做如下操作：1、拉取代码。2初始化本地仓库。3、创建别名

#### SSH免密登录

Windows家目录运行git bash

`ssh-keygen -t rsa -C` SSH链接

输入`cat ~/.ssh/id_rsa.pub`查看公钥

公钥添加进GitHub

