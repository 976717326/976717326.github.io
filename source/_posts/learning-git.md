---
title: Git 实战
date: 2018-11-20 10:21:10
tags:
  - git
categories:
  - git
toc: ture
---

本文用最简单的步骤创建本地仓库和远程仓库，并将其关联起来。

<!---more--->

# 方法一

## 创建本地仓库

1. 新建仓库文件夹，例如`learning-git`。

2. 在`learning-git`目录下打开 **git** 命令行界面，输入以下命令：

   ```bash
   git init
   ```

   正常情况，得到以下结果：![](/images/learning-git/git-init.jpg)

3. 在`learning-git`内加入`.gitignore`和**需要进行版本管理的源文件**（本例加入了test.c），请注意在`.gitignore`文件内包含需要忽略的文件。

   ![](/images/learning-git/learning-git-folder.jpg)

4. 将所有文件添加到仓库中，在命令行内输入：

   ```bash
   git add .
   ```

   执行该命令后无返回结果，可以通过`git status`查看仓库状态得到以下结果：

   ![](/images/learning-git/git-add.jpg)

5. 将修改提交到本地仓库：

   ```bash
   git commit -m "first commit"
   ```

   命令行结果如下：![](/images/learning-git/git-commit.jpg)

## 创建远程仓库

1. 登录git托管主页，本例以[码云](gitee.com)为例。创建步骤如下：

   ![](/images/learning-git/create-remote-repo.jpg)

2. 创建完成后，网页会自动跳转到一个新的页面。由于该远程仓库目前是空的，所以显示默认页面。

   ![](/images/learning-git/remote-repo-init-page.jpg)

3. 复制远程仓库的地址，请注意复制SSH地址，如果使用HTTPS地址，会导致后续在命令行窗口操作时（push等命令）需要输入账号密码：

   ![](/images/learning-git/git-ssh-addr.jpg)

## 关联远程仓库

1. 关联远程仓库，输入以下指令

   ```bash
   git remote add origin git@gitee.com:rhh/learning-git.git
   ```

   其中`origin`为远程仓库的名称，可以自由修改；`git@gitee.com:rhh/learning-git.git`为上一步骤中复制的远程仓库的地址。

2. 推送代码到远程仓库

   ```bash
   git push -u origin master
   ```

   成功后会提示相关信息：![](/images/git-push.jpg)

# 方法二

## 创建远程仓库

1. 登录git托管主页，本例以[码云](gitee.com)为例。创建步骤方法一步骤，其中不一致的地方如下：

   ![](/images/learning-git/create-remote-repo-alt.jpg)

   >  添加`.gitignore`或者`Readme`文件均可。

2. 创建完成后，网页会自动跳转到一个新的页面。由于该远程仓库创建时添加了`.gitignore`文件，所以创建时会自动添加一个*init commit*。

   ![](/images/learning-git/remote-repo-init-alt.jpg)

3. 复制远程仓库的地址，请注意复制SSH地址，如果使用HTTPS地址，会导致后续在命令行窗口操作时（push等命令）需要输入账号密码：![](/images/learning-git/clone-remote-repo.jpg)

## 克隆远程仓库

1. 在目标文件夹内打开`git`命令行，输入以下指令：

   ```bash
   git clone git@gitee.com:rhh/learning-git.git
   ```

   执行后会输出以下信息：

   ![](/images/learning-git/git-clone.jpg)

2. **进入`learning-git` 文件夹**，添加需要进行版本管理的源文件，例如`test.c`。

3. 将文件加入到本地仓库：

   ```bash
   git add .
   ```

4. 修改提交到仓库：

   ```bash
   git commit -m "add source files"
   ```

   输出如下：

   ![](/images/learning-git/git-commit-alt.jpg)

   由于原来就存在`.gitignore`文件，所以只提示添加了一个文件。

5. 推送到远程仓库：

   ```bash
   git push
   ```

   结果如下：

   ![](/images/learning-git/git-push-alt.jpg)