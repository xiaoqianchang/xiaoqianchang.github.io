title: Git常用命令
date: 2016-03-02 14:53:20
categories: Git
tags: 
  - Git
description: Git的常用命名集合，包括创建分支、合并分支、删除分支、tag的操作、reset的操作、撤销、解决commit冲突和删除分支上的文件及文件夹。
---

## 在Git下创建一个空分支

### 怎样安全的进行这项操作？

我们需要建立一个孤立的分支，为了尽可能的保证数据安全，最好重新clone一份代码。

``` python
$ git clone https://github.com/user/repo.git // Clone our repo
```

好了，代码clone好了。

### 开工

这里以github的操作为例，下面试图创建一个名为gh-pages的空分支

    $cd repo
    
    $ git checkout --orphan gh-pages
    # 创建一个orphan的分支，这个分支是独立的
    Switched to a new branch 'gh-pages'
    
    git rm -rf .
    # 删除原来代码树下的所有文件，包括.gitignore
    rm '.gitignore'

注意这个时候你用git    branch命令是看不见当前分支的名字的，除非你进行了第一次commit。
下面我们开始添加一些代码文件，例如这里新增了一个index.html

``` python
$ echo \"My GitHub Page\" > index.html
$ git add .
$ git commit -a -m \"First pages commit\"
$ git push origin gh-pages
```

在commit操作之后，你就可以用git branch命令看到新分支的名字了，然后push到远程仓库。

如果觉得这些命令记不到，推荐使用工具Git Extensions，在工具栏Commands-->Create branch...

## Git创建与合并分支

> 主干（master）作为所有开发人员的主干，也是所有项目的根（root），例如，目前“医路通”项目的整个进展应该都是在master中，包含一期、二期和三期的开发。
	分支（branch），依托于主干（master）延伸出来的版本需求，都可以创建分支。例如，医路通临时版本的发布，需要将部分功能屏蔽，这个时候就建议从主干中拉出来一个分支，在分支上修改，并最终发出版本，而不是直接拿主干来修改。
	标签（tag），tag的作用更多是作为备份和里程碑使用，如果我们发一个版本，一般都需要打一个tag的。
    软件开发过程中，同一个项目，常常会延伸不同的其他项目分支；又或者仅仅是为了发布而需要屏蔽某些功能的时候，我们就需要使用不同的分支来进行开发了。Git下如何快速建立分支呢？

### Git分支的创建

 - 查看分支

``` python
git branch
```

 - 创建分支

``` bash
git branch name
```

 - 切换分支

``` bash
git checkout name
```

 - 创建+切换分支

``` bash
git checkout -b name
```
- 创建分支后同步到服务器

``` bash
git push origin name
```

### Git分支的合并与提交

 - 合并某分支到当前分支

``` bash
git merge name
```

例如，如果要从其他分支合并到master的分支中，则需要先切换到mastert分支，然后再进行merge。

``` bash
git checkout master
git merger name
```

幸运的话，即可看到合并成功的提示。如果有所冲突，那么可能会提示你修改代码，这个时候不要慌张，根据提示，进入代码中，删除代码中“head *** ”之类的提示，保存后，重新运行merge命令，直到完成为止！

- 将git分支push到服务器即可

``` bash
git push origin name
```

### 删除分支

 - 删除分支

``` bash
git branch -d name
```

操作示例：
![Git](http://7xrcic.com1.z0.glb.clouddn.com/e062651b-2333-46d5-ab87-b31296b1017c.jpg)

### 本地分支与远程分支

 - 远程分支就是本地分支push到服务器上的时候产生的。如master就是一个最典型的远程分支（默认）

``` bash
git push origin master
```

 - 随便创建分支，然后push到服务器就生成的远程分支

``` bash
git checkout -b dev // 创建本地dev分支用于开发
git checkout -b bug // 创建本地bug分支用于bug处理
git checkout -b feature // 创建本地feature分支用于新功能开发

git push origin dev // 生成远程dev分支
git push origin bug
git push originfeature
```

 - 远程分支与本地分支的区分。在服务器上拉去特定分支时要指定本地分支名称

``` bash
git checkout --track origin/dev
```

注意该命令带有--track参数，所以要求git1.6.4以上！这样git会自动切换到develop分支

 - 同步本地远程分支

``` bash
git fetch origin
```

## Git的tag操作

标签（tag），tag的作用更多是作为备份和里程碑使用，如果我们发一个版本，一般都需要打一个tag的。

- 查看已有标签

``` bash
git tag
```

- 特定的搜索模式列出符合条件的标签

``` bash
git tag -l 'v1.4.2.*'
```

- 查看指定tag

``` bash
git show tag_name
```

- 创建简单标签

``` bash
git tag tag_name
```

- 创建带注释的标签

``` bash
git tag -a v1.0 -m '1.0milestone' // -m 后面是注释
```

- 推送[分享]标签，tag需要单独push

``` bash
git push origin -tags // 这样其他人也能看到标签了
```

## Git的reset操作

实际开发过程中，经常会遇到一些莫名其妙的问题，例如，明明跟服务器代码同步的，但是却提示有代码需要提交。诸如此类问题，就可以使用reset操作了。

``` bash
git reset --hard
```

git回到上一版本命令：
`git reset`是指将当前head的内容重置，不会留log信息
`git reset HEAD filename`  从暂存区中移除文件
`git reset --hard HEAD~3`  会将最新的3次提交全部重置，就像没有提交过一样

根据`--soft` `--mixed` `--hard`，会对working tree和index和HEAD进行重置：
`git reset --mixed`：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
`git reset --soft`：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
`git reset --hard`：彻底回退到某个版本，本地的源码也会变为上一个版本的内容
例如：
我要彻底返回在上一次提交以前的版本。`git reset --hrad HEAD~1`
将本地的状态回退到和远程的一样。`git reset –hard origin/master`
回退到上一次提交的状态，按照某一次的commit完全反向的进行一次commit。`git revert HEAD`

另外如果要舍弃本地新的文件，可以使用下边的命令：

``` bash
git clean -df
```

## Git删除分支和删除文件夹

### 删除分支

 - 查看所有分支

``` bash
git branch -a
```

 - 删除HEAD分支

``` python
git push origin --delete HEAD
```

### 删除文件夹

 - 查看本地分支下的文件

``` bash
ls
```

 - 删除xx文件夹及其下所有的文件

``` bash 
git rm xx -r -f
```

 - 同步删除操作到远程分支

``` bash 
git commit -m "delete xx"
```

 - 提交分支

``` bash
git push origin master
```

## Git的撤销与解决冲突

### Git的撤销

- git撤销本地修改

``` bash
git reset --hard origin/master
git pull
```

- git回退到前n个版本

``` bash
git reset --hard HEAD~3
```

### Git多用户提交冲突一

场景：用户UserA修改了文件File1，用户UserB也修改了文件File1并成功merge到了服务器上，而UserA和UserB改动了同一个代码块，当UserA拉取代码时git无法merge此改动，就会出现如下错误提示，
error: Your local changes to the following files would be overwritten by merge: cn/trinea/appsearch/MainActivity.java
Please, commit your changes or stash them before you can merge.
这时

 - 如果希望保存本地改动并拉下最新服务器代码，手动merge，使用命令如下：

``` bash
git stash
git pull
git stash pop
git diff -w cn/trinea/appsearch/MainActivity.java
```

其中git stash表示备份当前工作区内容到git栈中，并使当前工作区内容与上次提交时一致，然后git pull拉取最新代码，git stash pop表示从Git栈中读取最近一次保存的内容，恢复工作区的相关内容，最后git diff表示手动merge你之前冲突的文件

- 如果希望服务器上版本完全覆盖本地修改，使用如下命令回退并更新：

``` bash
git reset --hard
git pull
```
 
### Git多用户提交冲突二

场景：用户UserA提交了change A，没有merge，之后用户UserB提交了change B，merge成功。当merge change A时出错，会提示，
The change could not be merged due to a path conflict.
Please rebase the change locally and upload the rebased commit for review.
 
大多数人的解决方式都是拷贝改动代码，并重拉最新代码Beyond Compare，重新提交。其实几条命令就可以搞定，gerrit上先abandon原来的提交，后执行如下命令：

``` bash
git reset --hard HEAD~2
git pull
git fetch ssh://xxxx refs/changes/46/28146/1 && git cherry-pick FETCH_HEAD
git push gerrit:xxxxxx HEAD:refs/for/xxxxxx
```

其中git reset –hard HEAD~2表示本地代码后退两级，如果有问题可以多后退几次
git pull表示拉最新代码
git fetch 表示获取之前没merge成功的改动到本地，后面跟的具体地址为gerrit上该change review页面选择cherry-pick、ssh后的地址
git push 跟平时push一样

Enjoy It!

## 学习资料 ##

[Try Github在线学习Git版本控制](https://try.github.io)