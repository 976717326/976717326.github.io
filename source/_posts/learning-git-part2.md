---
title: Git 实战Ⅱ
date: 2018-11-27 22:35:10
tags:
  - git
categories:
  - git
toc: ture
---

本文介绍分支创建与切换。

<!---more--->


在使用git的过程中，不免会遇到版本退回、修改，然后重新提交，以及分支切换的情况。以一个例子为例：

当前所处分支为`master`。可以使用`git log`命令查看提交情况：

![](/images/learning-git-part2/git-log-init.jpg)

可以看出，`master`分支只有3个提交信息。

假设一下情景：由于测试的需要，需要退回到`commit 76bc4b8df59cb5b7647f7ecbcd3dbee1c5670ece`之后的状态，并且在此基础上进行修改和提交，以供测试使用。最后代码需要返回到`master`的初始的最新版本上继续开发。

## 思路如下：

在需要修改的提交上，新建相应的调试分支，然后在调试分支上修改并提交，最终返回到`master`分支。

## 步骤如下：

### 返回到需要修改的提交

在命令行界面输入命令：`git checkout 76bc4`，输出如下：

![](/images/learning-git-part2/git-first-checkout.jpg)

​	`76bc4`是上图需要退回到的提交的代码的前几个字母，提交代码在仓库内的标识是唯一的。一般如果仓库不是特别大，前几个字母足够唯一标示某一提交。输入此命令后，分支已经返回到`76bc4`提交之后的状态。

## 新建测试分支

输入命令：`git checkout -b test`，输出如下：

![](/images/learning-git-part2/git-create-branch.jpg)

## 修改文件并提交

修改源文件并保存后，在命令行界面输入命令：`git commit -a -m "do something"`，输出如下：

![](/images/learning-git-part2/git-commit.jpg)

可以看到此时已提交成功，利用`git log`查看修改记录：

![](/images/learning-git-part2/git-second-log.jpg)

说明该代码是在`commit 76bc4b8df59cb5b7647f7ecbcd3dbee1c5670ece`的基础上修改的，可将该版本提交到测试环节。

## 返回`master`分支

在测试完成后，需要返回到原来所在的分支，在命令行界面输入命令：`git checkout master`，输出如下：

![](/images/learning-git-part2/git-second-checkout.jpg)

测试代码已经返回到最初始的状态，可用`git log`查看提交记录：

![](/images/learning-git-part2/git-third-log.jpg)