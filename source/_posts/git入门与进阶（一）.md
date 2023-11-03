---
title: git——分布式管理系统的入门与进阶（一）
date: 2023-10-31 09:18:11
tags:
- git
- 分布式管理系统
categories: 版本控制系统
---

## 版本控制
什么是“版本控制”？它究竟有什么作用？

版本控制是一种系统，它记录了一个或者若干个内容变化，使得我们可以查阅各个版本的变化。

<!--more-->

有很多人会认为，只有源代码文件需要用到版本控制，但其实，你可以对任何类型的文件进行版本控制。

有了版本控制系统，我们就可以将项目或是某个文件回溯到过去某个时间点的状态，可以通过比较文件的变化细节。查出最后是谁的修改导致出现的问题。使用版本控制系统意味着，即使你将所有文件乱删乱改一气，也可以轻松恢复它之前的样子。

## 版本控制的发展历史
- 本地版本控制系统：其中一种为RCS (Revision control system)，在硬盘上保存补丁集，通过应用所有补丁可以计算出各个版本的文件内容。*目前RCS由GNU工程师维护*

- 集中化版本控制系统(Centralized Version Control Systems)：诸如CVS、Subversion以及Perforce，有一个集中管理的服务器，保存所有文件的修订版本。缺点是中心服务器单点故障会导致谁都无法使用，服务器的磁盘损坏时也会导致丢失项目的所有数据。

- 分布式版本控制系统(Distributed Version Control System)：Git即为其中一员。客户端并不只提取最新版本的文件快照， 而是把代码仓库完整地镜像下来，包括完整的历史记录。 每一次的克隆操作，实际上都是一次对代码仓库的完整备份。
## 两周时间，完成Git
2005 年，Linux 系统的代码库之大已经让 Linus 很难继续手工管理了。

于是，Linus 花了**两周**时间自己用 C 写了一个分布式版本控制系统，这就是 Git！一个月之内，Linux 系统的源码已经由 Git 管理了！
<img src="https://echobox-1307380937.cos.ap-shanghai.myqcloud.com/echo/202310311011327.jpg" alt="一个具有代表性的 git 会话，演示了存储库的初始化，以及文件和远程的添加。" style="zoom:50%;" />

诞生以来，Git 日臻完善，2008年，GitHub 网站上线了，它为开源项目免费提供 Git 存储，无数开源项目开始迁移至 GitHub .

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/1920px-Git-logo.svg.png" alt="定义" style="zoom: 25%;" />
## 安装Git
官方版本可以在 Git 官方网站下载。 打开 https://git-scm.com/download/win，然后选择相应的版本即可。 

**git config**

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。

接下来，我们先学习如何通过 `git config` 命令来配置 **用户信息**

### 配置用户名和邮件地址 
安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改： 

```bash 
$ git config --global  "你的用户名" 
$ git config --global user.email 你的邮箱 
```
再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 > 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。

如果想要检查你的配置，可以使用 `git config --list` 命令来列出所有 Git 当时能找到的配置。 

你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：`/etc/gitconfig` 与 `~/.gitconfig`）。 这种情况下，Git 会使用它找到的每一个变量的最后一个配置。 你可以通过输入 `git config ：` 来检查 Git 的某一项配置，例如： 

```bash 
$ git config user.name  
```
![Mona Lisa为作者设置的用户名，使用命令检查](https://echobox-1307380937.cos.ap-shanghai.myqcloud.com/echo/202310311050837.jpg)
## 更多Git 配置 

### Git 颜色配置

 到目前为止，我们已经配置了 `user.name` 和 `user.email` ，实际上，Git 还有很多可配置项。 比如，让 Git 显示颜色，会让命令输出看起来更醒目： 
 ```bash
 $ git config --global color.ui true 
 ```
 **Git 显示颜色**

### Git忽略文件配置
有些时候，你必须把某些文件放到 Git 工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件等等，每次`git status`都会显示`Untracked files ...`，这种情况下，就可以实用忽略特殊文件 `.gitignore` 来很方便的解决这个问题。 首先我们在 Git 工作区的根目录下创建一个特殊的 `.gitignore`文件，然后把要忽略的文件名填进去，Git 在每次进行提交的时候就会自动忽略这些文件。 

 ### Git 配置别名 
除了通过 配置忽略文件 来提高`git commit` 时的便捷性外，Git 中还有一种可以让大家在敲入 Git 命令时偷懒的办法——那就是配置 Git 别名。 
**配置 git status/commit/checkout/branch**
比如在使用`git status`命令时，我们可以通过配置别名的方式将其配置为`git st`，这样在使用时是不是就比输入 `git status`简单方便很多呢？ 我们只需要敲一行命令，告诉 Git，以后`st`就表示`status`： ```bash $ git config --global alias.st status ``` 当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`： 

**配置别名**
```bash 
$ git config --global alias.co checkout 
$ git config --global alias.ci commit 
$ git config --global alias.br branch 
```
配置完成以上别名后，以后提交就可以简写成：
```bash 
$ git ci -m "sth." 
```
`--global`参数是全局参数，也就是这些命令在这台电脑的所有 Git 仓库下都可以使用。 
### 配置 git reset HEAD file 
再比如`git reset HEAD file`命令，他可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个`unstage`操作，就可以配置一个`unstage`别名： 
```bash 
$ git config --global alias.unstage 'reset HEAD' 
```
当你敲入命令： 
```bash 
$ git unstage test.py 
```
实际上 Git 执行的是： 
```bash 
$ git reset HEAD test.py 
```
### 配置 git log -1 
配置一个`git last`，让其显示最后一次提交信息： 
```bash 
$ git config --global alias.last 'log -1' 
```
这样，用git last就能显示最近一次的提交：
```bash 
$ git last 
commit 
4aac6c7ee018f24d2dabfd01a1e09c77612e1a4e 
(HEAD -> master) 
Author: 
Miykael_xxm Date: 
Tue Nov 17 11:14:15 2020 +0800 
branch test 
```
### Git 配置文件
这些自定义的Git配置文件通常都存放在仓库的`.git/config`文件中： 
**全局配置文件**

```bash
$ cat .git/config 
[core] 
repositoryformatversion = 0 
filemode = true 
bare = false 
logallrefupdates = true 
ignorecase = true 
precomposeunicode = true 
[remote "origin"] 
url = git@codechina.csdn.net:codechina/learngit.git 
fetch = +refs/heads/*:refs/remotes/origin/* 
[branch "master"] 
remote = origin merge = refs/heads/master 
[alias] 
last = log -1 
```
别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。 而当前用户的 Git 配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中： 
**用户配置文件**

```bash 
$ cat .gitconfig 
[alias] 
co = checkout 
ci = commit 
br = branch 
st = status 
[user] 
name = Your Name 
email = your@email.com 
[color] 
ui = true
```
配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

