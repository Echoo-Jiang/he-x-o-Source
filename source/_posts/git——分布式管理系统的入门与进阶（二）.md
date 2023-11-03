---
title: git——分布式管理系统的入门与进阶（二）
date: 2023-10-31 13:38:55
tags:
- git
- 分布式管理系统
categories: 版本控制系统
---

### Git使用

认识一下版本库——Repository，接下来我们所有提到的 Git 基础命令，都是基于版本库的。

仓库，英文名 repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

<!--more-->

#### 在已存在目录中初始化仓库 —— git init

在已存在目录中创建一个版本库的过程非常简单：

首先，选择一个合适的地方，创建一个空目录：

**创建目录**



```bash
$ mkdir learning-git 
$ cd learning-git 
$ pwd /Users/xxm/learning-git
```

`pwd`命令用于显示当前目录。

第二步，通过`git init`命令把这个目录变成 Git 可以管理的仓库：

**初始化仓库**

进入到要将目录变成仓库的路径，然后

```bash 
$ git init 
```

瞬间 Git 就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），同时在当前目录下多了一个`.git`的目录，这个目录是 Git 来跟踪管理版本库的，如果你没有看到 .git 目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看到了。

## 编辑并添加文件
接下来，我们来尝试在已经准备好的 Git 仓库中创建一个readme.txt文件，内容如下：

```bash Git is a version control system. Git is free software.```

接下来，我们可以通过2个命令将刚创建好的readme.txt添加到Git仓库：

第一步，用命令git add告诉 Git，把文件添加到仓库：

```bash 
$ git add readme.txt
```
执行上面的命令，没有任何显示，说明添加成功。

提交变动到仓库
第二步，用命令git commit告诉 Git，把文件提交到仓库：

```bash 
$ git commit -m "随便写点说明wrote a readme file" 
[master (root-commit) 50ed06b] 
wrote a readme file 1 file changed, 2 insertions(+) create mode 100644 readme.txt
```
>这里简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

git commit命令执行成功后会告诉你：

- 1 file changed：1个文件被改动（我们新添加的readme.txt文件）
- 2 insertions：插入了两行内容（readme.txt有两行内容）
为什么 Git 添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：

```bash 
$ git add file1.txt 
$ git add file2.txt file3.txt 
$ git commit -m "这次一共add 3 files."
```

#### 查看Git仓库当前状态变化

我们已经成功地添加并提交了一个`readme.txt`文件，接下来让我们继续修改`readme.txt`文件，改成如下内容：

```
bash Git is a distributed version control system. Git is free software.
```
>添加了“distributed”

**查看 git status 结果**

现在，运行`git status`命令看看结果：

```bash 

$ git status 
On branch master Changes not staged for commit: (use "git add ..." to update what will be committed) (use "git checkout -- ..." to discard changes in working directory)
modified:   readme.txt
```
```
no changes added to commit (use "git add" and/or "git commit -a") 
```

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

#### 比较变动

虽然 Git 告诉我们`readme.txt`被修改了，但并没有告诉我们具体修改的内容是什么，假如刚好是上周修改的，等到周一来班时，已经记不清上次怎么修改的`readme.txt`，这个时候我们就需要用`git diff`这个命令查看相较于上一次暂存都修改了些什么内容了：

**运行 git diff 命令**



```bash 
$ git diff 
readme.txt 
diff --git a/readme.txt b/readme.txt 
index 46d49bf..9247db6 100644 
--- a/readme.txt 
+++ b/readme.txt 
@@ -1,2 +1,2 @@ 
-Git is a version control system. 
+Git is a distributed version control system. Git is free software. (END)
```

`git diff`顾名思义就是查看 difference，显示的格式正是 Unix 通用的 `diff` 格式，可以从上面的输出看到，我们在第一行添加了一个`distributed`单词。

#### 查看日志

git log可以查看全部的commit记录，如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

**git log --pretty=oneline**

## 克隆现有的仓库 —— git clone

如果你想获得一份已经存在了的 Git 仓库的拷贝，比如说，你想为某个开源项目贡献自己的一份力，这时就要用到 git clone 命令，Git 克隆的是该 Git 仓库服务器上的几乎所有数据，而不是仅仅复制完成你的工作所需要文件。

**git clone**


当你执行`git clone` 命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。

克隆仓库的命令是 git clone <url>。 比如，要克隆 Git 的链接库 libgit2，可以用下面的命令：

``` bash 
$ git clone https://gitcode.net/codechina/help-docs
```

这会在当前目录下创建一个名为 help-docs 的目录，并在这个目录下初始化一个 .git 文件夹， 从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。 如果你进入到这个新建的 help-docs 文件夹，你会发现所有的项目文件已经在里面了，准备就绪等待后续的开发和使用。

**自定义本地仓库名称**

当然如果你想在克隆远程仓库的时候，自定义本地仓库的名字也是可以的，你可以通过额外的参数指定新的目录名：

``` bash 
$ git clone https://gitcode.net/codechina/help-docs mydocs
```

这会执行与上一条命令相同的操作，但目标目录名变为了` mydocs`。

Git 支持多种数据传输协议。 上面的例子使用的是 https:// 协议，不过你也可以使用 git:// 协议或者使用 SSH 传输协议，比如 user@server:path/to/repo.git 。
