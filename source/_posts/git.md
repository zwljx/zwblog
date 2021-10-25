---
title: Git知识点总结
date: 2021-10-22 13:57:35
tags: git
categories: git
---
git操作总结

<!--more-->

### 初级

#### 工作流

![工作流](git.jpg)

working directory是操作目录
staging area是暂存区，用来保存变化
repository是仓库，保存作为一个版本的变化

#### 基础命令

git init
git status
git diff
git log
git reflog
git add [. | filenames]
git commit -m “message <=50 characters”

#### HEAD

HEAD:当前所在版本
HEAD^:上一个版本
HEAD~n:上n个版本
SHA：40 characters 唯一标识取至少7位

### 中级

#### 分支

git branch 当前分支
git branch new_branch 新建分支
git checkout branch_name 切换分支
git merge branch_name(提供者分支名称) 合并到当前分支
git branch -d branch_name 删除分支
注：若有冲突conflict，需要手动修改，再add、 commit

#### teamwork

git clone remote_location(from) clone_name(to)克隆远端的仓库到本地
git remote -v 查看远端仓库的版本信息
git fetch 查看远端修改（这个操作只是合并到了本地的origin/master,不是本地的local/master）
git push your_branch_name 上传到远端
注：
origin/master 标志远端HEAD的位置
git fetch区别于git pull：fetch不会自动merge，pull会自动merge。fetch更安全
使用Github需要先fork到自己的账号下，再从自己的账号下clone，才可以提交修改

#### 工作流程

一般的使用git来做版本控制的工作流程如下：
①fetch and merge （=pull）
②创建新分支来开发新的功能
③开发、commit
④fetch and merge
⑤push your branch to remote

### 高级

#### reset和checkout

这是与回退相关的两个常用命令，到底什么时候该用哪个命令，具体用法和区别如下：

reset命令一般用于版本回退：注意不只是可以做仓库的版本回退。使用如下：
命令格式：git reset [- -参数] [commit-id] [filename]
hard：HEAD INDEX WORKING都改变
mixed：HEAD INDEX改变（默认）
soft: HEAD改变
加文件名代表只有该文件做回退，不加代表所有文件均回退

checkout有两个作用：切换分支和撤销修改（指修改工作区，仓库的版本都还存在）
撤销修改：
①git checkout - -[.|filename]
若add过，回到add之后的状态。若没有add过，回到上一次commit
②工作区内容回到上一次或者上n次的版本
git checkout commit-id filename 或者 先reset在checkout - -

两种方式的对比：
前者容易误操作，但是保留了该文件工作区目标版本之后的几个版本。后者不容易误操作，但是几个版本会丢失。

关于前者的误操作和解决方式：
filename是必须加的，若不加，该命令将使HEAD指针不再指向最新的版本，而是处于游离状态。解决方法是：checkout到master分支，但若目前已有修改无法切换到master分支。则新建temp分支，commit再合并到master分支，然后删除temp分支。可以将HEAD指针回到最新版本的位置。

#### git stash

当前分支工作未完成，不能commit，但是要切换到其他分支去工作。问题是：不commit修改无法切换到其他分支。因此要用到stash。
git stash 备份
git stash pop 读取并恢复（pop命令之后不再有，apply之后还有）
git stash list
git stash clear 清空
git stash apply [stash @{1}]
git stash drop [< stash >]删除某进度，默认最新
注：不能保存untracked files，需要加-u参数

#### github的使用（摘了网上的）

配置Git

首先在本地创建ssh key；
ssh-keygen -t rsa -C “your_email@youremail.com”
后面的your_email@youremail.com改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在~/下生成.ssh文件夹，进去，打开id_rsa.pub，复制里面的key。
回到github上，进入 Account Settings（账户配置），左边选择SSH Keys，Add SSH Key,title随便填，粘贴在你电脑上生成的key。
github-account
为了验证是否成功，在git bash下输入：
ssh -T git@github.com
如果是第一次的会提示是否continue，输入yes就会看到：You’ve successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。
git config –global user.name “your name”
git config –global user.email “your_email@youremail.com”
进入要上传的仓库，右键git bash，添加远程地址：
git remote add origin git@github.com:yourName/yourRepo.git
后面的yourName和yourRepo表示你再github的用户名和刚才新建的仓库，加完之后进入.git，打开config，这里会多出一个remote “origin”内容，这就是刚才添加的远程地址，也可以直接修改config来配置远程地址。

创建新文件夹，打开，然后执行 git init 以创建新的 git 仓库。
检出仓库
执行如下命令以创建一个本地仓库的克隆版本：
git clone /path/to/repository
如果是远端服务器上的仓库，你的命令会是这个样子：
git clone username@host:/path/to/repository