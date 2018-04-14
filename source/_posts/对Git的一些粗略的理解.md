---
layout: post
title: 对Git的一些粗略的理解
date: 2017/11/20
---


# Git的目录结构

当你在某个git工程下使用ls -a 命令的时候就会发现当前目录多出了一个.git隐藏文件夹  

cd .git  进入该文件夹使用命令 ls  会发现一片新的天地，里头好些文件与文件夹。内容如下：

|    文件夹名     | 作用          
| ------------- |:-------------:
| HEAD     | 这个git项目当前处在哪个分支里
| config     | 项目的配置信息，git config命令会改动它      
| description | 项目的描述信息  
| index | 索引文件 
| hooks/ | 系统默认钩子脚本目录  
| logs/ | 各个refs的历史信息
| objects/ | Git本地仓库的所有对象 (commits, trees, blobs, tags) 
| refs/ | 标识你项目里的每个分支指向了哪个提交(commit) 


接下来找个有git管理的项目

![image](http://ojg2nfova.bkt.clouddn.com/1.png)

进入HEAD找到目前被检出的分支
vim HEAD

内容如下：ref: refs/heads/master

跟随着他的指示我们继续前往refs/heads目录

![image](http://ojg2nfova.bkt.clouddn.com/2.png)

git cat-file: 命令显示版本库对象的内容、类型及大小信息

通过 cat-file 命令可以将数据内容取回。该命令是查看 Git 对象的瑞士军刀。传入 -p 参数可以让该命令输出数据内容的类型：

![image](http://ojg2nfova.bkt.clouddn.com/3.png)

tree:指向一个tree对象，至于tree对象是什么后面有说
parent：我猜是他的上一个提交，至于是不是使用git log命令来验证下

![image](http://ojg2nfova.bkt.clouddn.com/4.png)

通过git log 发现parent指向的确实是上一个提交。

author
committer 
这两个不用看也知道是啥了

先看看 56c4433155f3e1b1321555053908f98434c383d3 这个末置点，

返回.git目录然后进入object,可以在 objects 目录下看到一个文件。这便是 Git 存储数据内容的方式──为每份内容生成一个文件，取得该内容与头信息的 SHA-1 校验和，创建以该校验和前两个字符为名称的子目录，并以 (校验和) 剩下 38 个字符为文件命名 (保存至子目录下)。

git cat-file -p 56c4433155f3e1b1321555053908f98434c383d3

![image](http://ojg2nfova.bkt.clouddn.com/5.png)

好巧，与refs/heads/master的内容可以说是一模一样的了，由此可见一大串的SHA指的就是一次commit，一个commit存储着文件改动以及他的父节点

说下git中各种对象的意义，每个对象(object) 包括三个部分：类型，大小和内容。大小就是指内容的大小，内容取决于对象的类型，有四种类型的对象："blob"、"tree"、 "commit" 和"tag"：

* “blob”用来存储文件数据，通常是一个文件。
* “tree”有点像一个目录，它管理一些“tree”或是 “blob”（就像文件和子目录）
* 一个“commit”只指向一个"tree"，它用来标记项目某一个特定时间点的状态。它包括一些关于时间点的元数据，如时间戳、最近一次提交的作者、指向上次提交（commits）的指针等等。
* 一个“tag”是来标记某一个提交(commit) 的方法。

### Blob对象

一个blob通常用来存储文件的内容.

你可以使用git show命令来查看一个blob对象里的内容。假设我们现在有一个Blob对象的SHA1哈希值，我们可以通过下面的的命令来查看内容：

![image](http://ojg2nfova.bkt.clouddn.com/6.png)


因为blob对象内容全部都是数据，如两个文件在一个目录树（或是一个版本仓库）中有同样的数据内容，那么它们将会共享同一个blob对象。Blob对象和其所对应的文件所在路径、文件名是否改被更改都完全没有关系。这里存放的是我第一篇博客的内容，可以去最前头找下。

### tree

一个tree对象有一串(bunch)指向blob对象或是其它tree对象的指针，它一般用来表示内容之间的目录层次关系。git show命令还可以用来查看tree对象，但是git ls-tree能让你看到更多的细节。如果我们有一个tree对象的SHA1哈希值，我们可以像下面一样来查看它：

![image](http://ojg2nfova.bkt.clouddn.com/7.png)

就如同你所见，一个tree对象包括一串(list)条目，每一个条目包括：mode、对象类型、SHA1值 和名字(这串条目是按名字排序的)。它用来表示一个目录树的内容。

一个tree对象可以指向(reference): 一个包含文件内容的blob对象, 也可以是其它包含某个子目录内容的其它tree对象. Tree对象、blob对象和其它所有的对象一样，都用其内容的SHA1哈希值来命名的；只有当两个tree对象的内容完全相同（包括其所指向所有子对象）时，它的名字才会一样，反之亦然。这样就能让Git仅仅通过比较两个相关的tree对象的名字是否相同，来快速的判断其内容是否不同。

若是用图来表示的话如下所示：

![image](http://ojg2nfova.bkt.clouddn.com/8.png)

### Commit对象

"commit对象"指向一个"tree对象", 并且带有相关的描述信息.
可以用 --pretty=raw 参数来配合 git show 或 git log 去查看某个提交(commit):

![image](http://ojg2nfova.bkt.clouddn.com/9.png)



可以看到, 一个提交(commit)由以下的部分组成:

* 一个 tree　对象: tree对象的SHA1签名, 代表着目录在某一时间点的内容.

* 父对象 (parent(s)): 提交(commit)的SHA1签名代表着当前提交前一步的项目历史. 上面的那个例子就只有一个父对象; 合并的提交(merge commits)可能会有不只一个父对象. 如果一个提交没有父对象, 那么我们就叫它“根提交"(root commit), 它就代表着项目最初的一个版本(revision). 每个项目必须有至少有一个“根提交"(root commit). 一个项目可能有多个"根提交“，虽然这并不常见(这不是好的作法).

* 作者 : 做了此次修改的人的名字,　还有修改日期.

* 提交者（committer): 实际创建提交(commit)的人的名字, 同时也带有提交日期. TA可能会和作者不是同一个人; 例如作者写一个补丁(patch)并把它用邮件发给提交者, 由他来创建提交(commit).

* －注释 用来描述此次提交.

注意: 一个提交(commit)本身并没有包括任何信息来说明其做了哪些修改; 所有的修改(changes)都是通过与父提交(parents)的内容比较而得出的. 值得一提的是, 尽管git可以检测到文件内容不变而路径改变的情况, 但是它不会去显式(explicitly)的记录文件的更名操作.　(你可以看一下 git diff 的 -M　参数的用法)

一般用 git commit 来创建一个提交(commit), 这个提交(commit)的父对象一般是当前分支(current HEAD),　同时把存储在当前索引(index)的内容全部提交.


一个commit的内容就如图所示：

![image](http://ojg2nfova.bkt.clouddn.com/10.png)

#git 一些常用初级命令


|    操作     | 作用          
| ------------- |:-------------:
| Git clone     | 从远程库下载一份代码
| Git branch     | 查看本地的分支      
| Git checkout | 切换分支 
| Git pull | 从远程拉代码到本地并与本地代码合并 
| Git push | 将一个commit提交到远程库 
| Git status | 查看当前的git版本库状态
| Git diff | 查看修改的内容
| Git reset | 将当前的分支重设（reset）到指定的<commit>或者HEAD（默认，如果不显示指定commit，默认是HEAD，即最新的一次提交）

#git 一些中阶命令

## git blame

如果你要查看文件的每个部分是谁修改的, 那么 git blame 就是不二选择. 只要运行'git blame [filename]', 你就会得到整个文件的每一行的详细修改信息:包括SHA串,日期和作者:

给具体文件执行$git blame后的效果如下:

![image](http://ojg2nfova.bkt.clouddn.com/11.png)

##Git revert

git revert 撤销 某次操作，此次操作之前和之后的commit和history都会保留，并且把这次撤销
作为一次最新的提交
    * git revert HEAD                  撤销前一次 commit
    * git revert HEAD^               撤销前前一次 commit
    * git revert commit （比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff）撤销指定的版本，撤销也会作为一次提交进行保存。
git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去，
版本会递增，不影响之前提交的内容

## Git stash/pop

git stash用于将当前工作区的修改暂存起来，就像堆栈一样，可以随时将某一次缓存的修改再重新应用到当前工作区。

用法:git stash 可用来暂存当前正在进行的工作， 比如想pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。
基础命令：
$git stash
$do some work
$git stash pop







