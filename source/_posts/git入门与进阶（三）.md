---
title: git入门与进阶（三）
date: 2023-10-31 15:44:27
tags:
- git
- 分布式管理系统
categories: 版本控制系统
---

## 版本回退

> 游戏里的save和load，git也能做到

<!--more-->

现在，你已经学会了修改文件，然后把修改提交到Git版本库，现在，再练习一次，修改readme.txt文件如下：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

然后尝试提交：

```
$ git add readme.txt
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

像这样，你不断修改文件，然后不断提交修改到版本库里，就好比游戏中的存档，打不过可以load重来。Git也是一样，每当你觉得文件需要一个“存档”时，就可以“保存一个快照”，这个快照在Git中被称为`commit`（类比成游戏的存档点）。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用上一章提到的`git log`命令查看，你会看见每个commit后面都有一串乱码，这就是版本号。

![image-20231031161459899](https://echobox-1307380937.cos.ap-shanghai.myqcloud.com/echo/202310311614958.png)

由于Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。所以commit后面的这串乱码其实就是SHA1计算出来的一个非常大的16进制数，可以防止版本号冲突。

好了，现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add a readme`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`f22c...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`append GPL`回退到上一个版本`add a readme`

就可以使用`git reset`命令(--hard参数的意义之后会讲，这里可以直接用)：

```
$ git reset --hard HEAD^
HEAD is now at 05b0d73 add a readme
```

![image-20231031161735214](https://echobox-1307380937.cos.ap-shanghai.myqcloud.com/echo/202310311617241.png)

看看`readme.txt`的内容,已经被还原了（读档了）。

此时再使用git log，会发现新版本append GPL已经消失了。怎么办？

只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`f22c2...`，于是就可以指定回到未来的某个版本：

依然还是`git reset --hard`只不过这次后面加的是版本号（不必写全版本号，取前几位，git会自动寻找），如下：
`$ git reset --hard f22c2`
运行完后再次查看readme.txt，果然，又找回了最新版本append GPL。

## 查看历史
现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？

在Git中，总是有后悔药可以吃的。当你用$ git reset --hard HEAD^回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。Git提供了一个命令git reflog记录了你的每次命令。
在这里给读者留下一个作业，使用git reflog命令找到之前的最新版本号，并恢复到最新版本。

