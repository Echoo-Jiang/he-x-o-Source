---
title: git入门与进阶(六)
tags:
  - git
  - 分布式管理系统
  - 撤销修改
  - 删除
categories: 版本控制系统
date: 2023-11-01 16:57:39
---
人非圣贤，都会犯错。现在Echo就闯了一个大祸，他不小心~~故意~~在`readme.txt`中添加了一行“我不想干活啦”。

<!--more-->

添加完后，还没有__add到暂存区__,echo就后悔了，因为老板很有可能帮助echo实现愿望。
不过现在事情还不算糟。ECho使用git status查看状态~~有没有留下痕迹~~，

![image-20231101202134867](https://echobox-1307380937.cos.ap-shanghai.myqcloud.com/echo/202311012021917.png)

可以看到git这里提示Echo使用“git restore”指令来丢弃工作区的更改。*老版本使用的则是checkout*

很好，echo知道这个指令以后马上就去尝试了。

他输入`git restore readme.txt`，回车。

果然，readme的内容已经变回去了。git status也没发现未提交的更改。

看来echo还可以继续在公司干一段时间。

但是假如，另一个时空的echo，不仅将这段话加入到了readme中，还不小心把更改add到了暂存区中。

万幸，还没**commit**。

echo~~又~~ 使用了git status。

![image-20231101203831656](https://echobox-1307380937.cos.ap-shanghai.myqcloud.com/echo/202311012038685.png)

贴心的git又给出了小tip。

echo这回使用

```bash
git restore --staged readme.txt
```

然后git status察看一下。

嗯，这下又回到了上个宇宙的echo的情况了。虽然这个时空的echo还是一头雾水，但我们的读者全都知道，再使用一次git restore readme.txt就可以了。

嗯，另另一个平行宇宙的echo，这回不仅**add**、还**commit**了，但他想起了**git 入门与进阶（五）**的内容，所以也无事发生。

什么？另另另一个平行宇宙的echo，这回不仅**add**、**commit**了，还提交到了远程版本库！

于是老板实现了echo不想工作的愿望。

## 删除文件

在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件`test.txt`到Git并且提交：

```
$ git add test.txt

$ git commit -m "add test.txt"
[master b84166e] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```
$ rm test.txt
```

现在git status查看一下状态

```bash
$ git status
On branch master
Changes not staged for commit:
(use "git add/rm <file>..." to update what will be committed)
(use "git restore <file>..." to d
```

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：通过提示use "git restore <file>得知，还是使用git restore，取消删除。
