---
title: git 简明教程
date: 2018-08-19 10:16:52
tags:
	- git
	- tutorial
categories:
	- Tutorial
	- git
---

git 是世界上最先进的版本控制系统。基于 git 的代码仓库，最著名的要属 Github 了，考虑到速度，国内的可以使用[码云](https://gitee.com/)或 [Coding](https://coding.net/)。

git 功能强大、命令丰富，一开始我们不可能，也不需要掌握全部。最好的方法是快速入门，在使用的过程中慢慢强化。这篇文章就是一个极简的入门教程。
<!-- more -->

## 安装

一如既往地安利 Linux 系统！对于个人电脑而言，Windows 操作系统是主流，确实简单、易用，我在公司和家里的笔记本上跑的都是 Windows 系统。但 Linux 可以赋予程序员更强大的能力，更何况你操作服务器总得用 Linux 吧。Win10 推出的 Linux 子系统（WLS）可以让你在 Win10 系统中方便地使用原生的 Linux 系统，强烈推荐！

如果已经有了 Linux 环境，git 很可能已经存在于你的系统中。可以在终端敲 git 命令检验。如果没有的话，可以很方便地通过各 Linux 发行版的包管理工具安装。以 Ubuntu 为例：

```bash
sudo apt install git
```

## 建库

从实用的角度解释：你在自己电脑上建个存放代码或文件的文件夹，这个文件夹叫做**本地仓库**。当你换台电脑工作时，不需要用 U 盘将文件夹拷来拷去，而是把代码上传到互联网上（云端），互联网上存放你代码或文件的地方叫做**远程仓库**。上面提到的 Github、码云和 Coding 等，就是给大家提供存放远程仓库的地方，这叫做**代码托管**。当然，它为无数的用户服务，每个用户可以建立很多个仓库。

以 Github 为例。首先注册 Github，[添加SSH Key](https://help.github.com/articles/connecting-to-github-with-ssh/)（不用每次都输密码）。

先在 Github 上建好远程仓库，比如叫 test。接下来有两种方式：一是克隆这个仓库到本地，二是将本地仓库连接到这个远程仓库。

### 克隆到本地

```bash
git clone git@github.com:jia-zhuang/test.git
```

这样会生成一个 test 文件夹，在该文件夹下工作就可以了。

### 将本地仓库连接到远程

```bash
# 将本地项目初始化为一个 git 仓库
git init

# 将目录下所有文件添加到本地仓库
git add *
git commit -m 'init and add all files'

# 连接远程仓库
git remote add origin git@github.com:jia-zhuang/test.git

# 将本地仓库同步到远程仓库
git push origin master
```

显然，如果想新建一个新项目，第一种方式更方便；如果项目已存在，可通过第二种方式托管到云端。


## 跟踪文件

```bash
# 查看状态
git status

# 查看提交记录
git log

# 添加文件跟踪状态
git add file.txt

# 删除文件跟踪状态
git rm file.txt
```

**.gitignore**

在项目目录下创建 .gitignore 文件，可以设置不跟踪的文件或文件夹，格式如下

```
*.o	# 不跟踪扩展名为 o 的文件
tmp/	# 不跟踪 tmp/ 目录下的文件
```


## 分支

```bash
git branch	# 显示本地分支
git branch  --all  	# 显示本地分支，包括远程绑定 
git branch -r 	# 显示远程分支

git branch <new-branch>		# 创建新分支
git checkout <branch>	# 切换分支
git checkout -b <new-branch>  # 创建新分支并切换到该分支

git merge <branch>  # 将 branch 分支合并到当前分支
```

## 管理远程仓库

```bash
git remote -v  # 显示
git remote add <name> <url>  # 添加，name 相当于给远程仓库起个名，指代 url 所指向的仓库
git remote rm  # 删除
```

下面给出几个实用性操作展示：

- 在本地创建一个新分支，并同步到远程仓库

	```bash
	git checkout -b new-branch
	git push origin new-branch
	```

	这样，远程仓库就会出现一个 new-branch 的新分支

- 将本地 master 分支推送到远程仓库的 another-branch 分支

    ```bash
    git push origin master:another-branch
    ```