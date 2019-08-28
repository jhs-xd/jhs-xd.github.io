---
title: Learn to Git
copyright: true
categories: 
  - hello git
tags: 
  - git

---
<blockquote class="blockquote-center">Sharp tools make good work.![](/myimages/7.jpg)</blockquote>

<!--more-->

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=320 height=86 src="//music.163.com/outchain/player?type=2&id=27093264&auto=0&height=66"></iframe>

---

# Git是什么？ #
Git是目前最先进的分布式版本控制系统
- 和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库
- Git相比于其他版本管理系统具有极其强大的分支管理功能
- Git最大的优势是远程仓库的功能，Github可以提供Git仓库托管服务

---

# 安装Git #
官网下载安装[Git](https://git-scm.com/download/win)
安装完成后，在开始菜单里找到"Git"->"Git Bash Here"，弹出一个命令行窗口就说明你的Git安装成功啦！
还需做以下简单设置：
- 第一步：设置global参数。因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址
		$ git config --global user.name "Your Name"
		$ git config --global user.email "email@example.com"
- 第二步：创建版本库
（1）创建一个目录。版本库创建完成后这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”
（2）通过git init命令把这个目录变成Git可以管理的仓库。当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的
- 第三步：把文本文件（.txt/.html/程序源码）添加到版本库
（1）用命令git add告诉Git，把文件添加到仓库：
		$ git add readme.txt
（2）用命令git commit告诉Git，把文件提交到仓库：
		$ git commit -m "wrote a readme file"

---

# git status #
git status命令可以让我们时刻掌握仓库当前的状态(4种)
1. 未添加文件到仓库（暂存区）
untracked
2. 修改了，未添加新的修改到仓库（暂存区）
![](/myimages/7GitStatus2.png)
3. 将修改添加到暂存区了，待提交
![](/myimages/7GitStatus3.png)
4. 没有可以提交的修改
![](/myimages/7GitStatus4.png)

---

# 其他git命令 #
1. git diff顾名思义就是查看difference（具体修改了什么内容），显示的格式正是Unix通用的diff格式
		$ git diff readme.txt
2. git log命令显示从最近到最远的提交日志，弹出commit id（版本号），后面可以加上参数  --pretty=oneline
3. git reset
回退到上一个版本（回到历史）： 
		$ git  reset --hard HEAD^
跳到指定版本号（穿梭到未来（需要版本号）、回到历史）： 
		$ git reset --hard 3628164
4. git reflog用来记录你的每一次命令，用它可以找到以前的版本号

---

# 工作区、暂存区（stage）、分支（master）#
![](/myimages/7GitStage.png)
我们把文件往Git版本库里添加的时候，是分两步执行的：
- 第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区
- 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支



> git commit只负责把暂存区的修改提交了（每次修改，如果不add到暂存区，那就不会加入到commit中）

---

# 撤销和删除 #

## 撤销修改 ##
- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令：git checkout -- file
- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作
- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，可以选择回退到之前的版本git reset --hard HEAD^，不过前提是没有推送到远程库

## 删除文件步骤 ##
一般情况下，你通常直接在文件管理器中把没用的文件删了
- 情景一：确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
- 情景二：删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本git chechout --file


> git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

---

# 远程仓库GitHub #

## 创建并添加SSH Key ##
本地Git仓库和GitHub仓库之间的传输是通过SSH加密的
- 创建SSH key

	$ ssh-keygen -t rsa -C "youremail@example.com"
- 添加SSH key
登陆GitHub，打开“Account settings”——>“SSH Keys”页面——>
点“Add SSH Key”，填上任意Title——>在Key文本框里粘贴id_rsa.pub文件的内容

## 链接本地git版本库和远程github版本库 ##
现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得
1. 首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：名称与本地git仓库最好一致
2. 将本地仓库与github仓库关联：
		$ git remote add origin git@github.com:github账户名/github上的仓库名.git
添加后，远程库的名字就是origin
3. 把本地库的所有内容第一次推送到远程库上：
		$ git push -u origin master
注意：
（1）把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程
（2）由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
4. 推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样
5. 从现在起，只要本地作了提交，就可以通过命令：$ git push origin master 把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

## 删除已有远程库origin ##
1. 本地库已经关联了一个名叫origin的远程库，此时，可以先用git remote -v查看远程库信息
2. 我们可以删除已有的GitHub远程库：git remote rm origin
