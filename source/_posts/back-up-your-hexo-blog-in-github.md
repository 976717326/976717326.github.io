---
title: 如何在github上备份自己的博客
date: 2017-12-24 14:17:55
toc: true
categories:
  - hexo
tags:
  - hexo
  - 博客备份
---

## 前言

很久以来都有一个想法，就是搭建属于自己的博客。网上也浏览了一些资料，发现现在线程的博客框架非常多，比如[wordpress](https://cn.wordpress.org/)、[hexo](https://hexo.io/)之类的。简单比对之后，选择了hexo框架，原因很简单：geek感十足，完全可以满足装逼的需要。

<!---more--->

## hexo博客搭建与部署

一般来说，大家都是利用hexo配合[github pages](https://pages.github.com/)来搭建博客的，为什么用github pages，原因很简单：github pages不收费！虽然github pages只能建立一个个人主页，但是足够使用了。关于博客的搭建过程，网上已经有数不清的教程，比如：
- [手把手教你使用Hexo + Github Pages搭建个人独立博客](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)
- [使用 Github 空间搭建 Hexo 博客 1–安装篇（基于 IntelliJ IDEA）](http://www.youmeek.com/hexo-install/)
- [可能是最详细的 Hexo + GitHub Pages 搭建博客的教程](http://www.lovebxm.com/2017/05/30/buildBlog/)

博客搭建完成之后，就是自己写文章，然后发布就行了，无非就是几条命令：

- 写文章：

```bash
hexo new "post_title"	# post_title就是新文章的链接标题
```

- 文章生成：

```bash
hexo clean && hexo g	# 连接符&&可以用来顺序执行连续的命令
```

如果需要现在本地预览文章效果的话，输入以下命令：

```bash
hexo server
```

打开浏览器，地址栏输入`localhost:4000`就可以预览文章最终的效果，如果觉得预览没有问题，就可以进行最终的部署。

- 最终部署：

```bash
hexo d	# d是deploy的缩写
```

最终部署会将文件推送到github的仓库里，过几分钟访问`<github_username>.github.io`就可以看到实际的效果。同时网友也可以通过该网址浏览自己的blog了。

## 博客备份

搭建好hexo博客之后，就已尽情地编写自己博客了。但是我突然意识到一个问题，hexo博客框架本身是一个静态的站点生成器，需要依托于本地的资源才能完成部署。那么问题来了：如果自己不小心把本地的文章删除了，接着还执行了`hexo clean && hexo g && hexo d`命令，那就很刺激了。如果回收站没有清空的话，还是可以抢救一下的，但是如果回收站清空了，GG！

相信很多人都遇到这个问题了，所以我去google了一下，还真发现了解决方案，用来部署博客的插件[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)的最新版集成了推送本地资源到远程仓库的功能。使用方法也很简单。

- 安装最新版的插件：.

对应npm版本低于4的，用以下命令来安装：

```bash
npm install git+git@github.com:hexojs/hexo-deployer-git.git --save
```

对应npm版本为5的，用以下命令来安装：

```bash
npm install git+ssh://git@github.com:hexojs/hexo-deployer-git.git --save
```

- 修改根目录下的配置文件`_config.yml`：

```
deploy:
  - type: git
    repo: git@github.com:github_username/github_username.github.io.git
    branch: master
  - type: git
    repo: git@github.com:github_username/github_username.github.io.git
    branch: src
    extend_dirs: /
    ignore_hidden: false
    ignore_pattern:
       public: .
```

这样，github的远程仓库就存在两个分支：
- `master`：用来存储构建网站所需要的资源
- `src`：用来存储本地资源

经过以上操作，这时使用`hexo d`命令，就会自动完成对`master`分支和`src`分支的更新。


## 其他问题

然而这个方案依然存在明显的问题：

- **一些私密的`id`和`token`可能会被公布到网上，存在安全隐患。在主题配置文件`_config.yml`中，可能存在一些用户的隐私数据，比如`livere_uid`和`leancloud_visitors`的相关信息。**

解决方案就是放弃`hexo-deployer-git`插件提供的自动备份功能，自己搭建一个私密仓库，专门用来备份自己的本地资源。然而我太懒了，不想再折腾了。我只想安静写个博客。。。然而。。就在我不想折腾的时候，又发现了一个问题：

- **七牛云的本地资源也被备份了！**

{% qnimg qiniuyun-folder.png title:七牛云资源文件夹 extend:?imageView2/2/w/600 %}

我使用七牛云插件的本意就是不让远程仓库保留`cdn`文件夹内的所有资源，让`cdn`文件夹内的资源都保存在七牛云的云端，从而节省github的空间。备份`cdn`文件夹完全违背了我的本意。。不能忍。。。

注意到根目录下的`_config.yml`文件的配置，我猜测可以添加一个选项来完成`cdn`文件夹的忽略：

```bash
deploy:
  - type: git
    repo: git@github.com:github_username/github_username.github.io.git
    branch: master
  - type: git
    repo: git@github.com:github_username/github_username.github.io.git
    branch: src
    extend_dirs: /
    ignore_hidden: false
    ignore_pattern:
        public: .
        cdn: .		# <-这里添加了cdn文件夹
```

事实证明，该设置没有任何作用。。厚着脸皮去请教了别人，终于得到了解答：

- **修改根目录内的`.gitignore`文件即可**

编辑`.gitignore`文件，在`.gitignore`文件的最后一行追加自己需要隐藏的目录或者文件：我这里设置了忽略`cdn`文件夹。

```bash
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
#追加cdn文件夹↓
cdn/	
```

这样，每次推送的时候就不会把`cdn`文件夹也推送到远程仓库了，大功告成！

## 后续工作

写到这里，基本可以满足我写博客的需求了。然而，一个人懒起来是很恐怖的。。为什么每次都要自己使用`hexo deploy`命令部署，不能自动化完成吗？

网上已经有现成的解决方案了：github+ [Travis CI](https://travis-ci.org/) 完成自动部署！相关教程也有还能多，google一下就有一堆博文详细介绍整个自动化部署流程。完成这个流程的意义就是：只需要一个命令就可以完成所有工作：

```bash
git push origin src	# src就是存放本地资源的分支
```

我也去尝试了一下，结果失败了。意料之中。。

另外由于我使用了七牛云，并且我将`cdn`文件忽略了，所以就算部署成功了，博文里面链接到七牛云的资源应该也会有问题。。除非我把`cdn`文件也一同上传，才有可能链接成功。（以上纯属猜测）

总结下来就一句话：生命不息。。折腾不止。。有空继续。。
