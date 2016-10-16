title: Hexo 搭建个人博客 · 基础篇
date: 2014-08-29 22:35:13
tags: Hexo
categories: Hexo
toc: true
---
Github 为了更好的程序员介绍自己的项目，提供了 Pages 功能，Pages 功能就是为每个项目提供一种静态网页的描述方式，很多网友就利用 Pages 功能搭建了自己的个人博客，这篇文章就是介绍如何使用 Hexo 快速的在 Github 上搭建自己的个人博客。

> Github 的 Pages 功能是免费的，省去了直接搭建博客的服务器成本；但是也有一定的限制，免费用户空间上线是 300M, 只能建立静态的个人博客，如果要建立动态网站还是用 WordPress 比较靠谱

<!-- more -->

## 工具介绍

### Hexo

Hexo 是一个基于 Node.js 的静态博客程序，可以方便的生成静态网页托管在 Github 和 Heroku上。作者是来自台湾的[@tommy351](https://github.com/hexojs/hexo)。官方介绍：
>A fast, simple & powerful blog framework, powered by Node.js.
>基于 Node.js 的快速、简洁但功能强大的博客框架。

Hexo 在 Gihub 上的主页是：[https://github.com/hexojs/hexo](https://github.com/hexojs/hexo)

### Node.js
由于使用 Hexo 需要依赖 Node.js, 所以对此进行简单的介绍，直接引用百度百科的内容：
>Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台，用来方便地搭建快速的易于扩展的网络应用. Node.js 借助事件驱动，非阻塞 I/O 模型变得轻量和高效， 非常适合运行在分布式设备的数据密集型的实时应用. Node.js 对 Google V8 引擎(应用于 Google Chrome 浏览器)进行了封装.

### Github
因为 Hexo 要将网站托管在 Github 上，也简单介绍下：
>Gist 是 Github 推出的基于 Git 的代码片段服务。用户可以提交自己的代码片段或任意的文本作，可以作为个人的代码管理库、文档管理库等。

如果没有用过 Github, 先到 [http://www.github.com](http://www.github.com) 上去注册一个账户，托管博客要用到。

### Git
Github 是 Git 的托管仓库，虽然用不到 Git 命令，但是 Hexo 也是依赖 Git 的。
>Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。[4] 
Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
Torvalds 开始着手开发 Git 是为了作为一种过渡方案来替代 BitKeeper，后者之前一直是 Linux 内核开发人员在全球使用的主要源代码工具。

如果使用 Windows 的童鞋，还请先到 [http://git-scm.com/](http://git-scm.com/) 下载 Git 客户端，Mac 本身自带 git, 用 Linux 的童鞋应该是比较专业的，就不介绍了。

## 搭建博客：
在使用 Hexo 之前，请先安装 Node.js 以及 Git，Node.js 和 Git 都有安装包，可以到官网去下载：
>Node.js 官网：[http://www.nodejs.org/](http://www.nodejs.org/)
>Git 官网：[http://git-scm.com](http://git-scm.com)

另外说明下：因为手头只有一台 Mac，所以下面使用的命令都是在 Mac 终端的，如果使用 Windows 的话，Git 有个 *Git Bash* 的程序，是一个模拟的终端，可以执行一些命令。

### 安装 Hexo
打开终端，执行命令：
````sh
$ npm install hexo -g
````

Hexo 安装成功之后，在任意目录新建一个子目录并切换到这个目录，这就是本地的博客目录：
````sh
$ cd ~
$ mkdir MyBlog
$ cd MyBlog
````

初始化本地博客目录：
````sh
$ hexo init
$ npm install
````

至此，本地的博客就搭建好了，是不是超简单 ^_^！
如果要本地查看博客，继续执行下面的命令：
````sh
$ hexo server   // 启动本地服务器
````
然后打开浏览器，在地址栏输入：localhost:4000，你就可以喉嗨尚的看到你的博客了～～～
>按下快捷键：`CTRL + D` 可以退出本地服务器

## 托管博客到 Github
尽管在本来浏览器中可已访问自己的博客了，但这仅仅只是一个单机版。如果要在整个互联网上发布直接的博客，需要有服务器托管，下面介绍如果托管直接的 Hexo 博客到 Github 上。登录 Github，创建已仓库(repositories), 名字为：`USERNAME`.github.io，如图：

![github_create](/img/github_create.png)

仓库创建好之后，可以查看；如上图 `Your repositories` 对应的列表中点击 *rakkang.github.io* 可以打开仓库主页。要上传项目到仓库，还要在 Github 的 `Settings` 中添加自己的 SSH Key。

![github_sshkey](/img/github_sshkey.png)

Github 仓库建好，设置项配置好之后，打开终端，进入博客目录：
````sh
$ cd ~/MyBlog
````

生成博客的静态网页，并部署到 Github:
````sh
$ hexo generate // 生成静态网页
````

在部署之前，还要配置下 Hexo 的服务器地址，打开 `~/MyBlog/_config.yml` 在文件的最后面，找到并编辑以下的内容：
````yaml
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: github    ------> 直接填写 github
  repository: https://github.com/rakkang/rakkang.github.io.git   -----> Github 的仓库地址
  brach: master
````

保存，在终端运行以下部署命令：
````shell
$ hexo deploy
````
>Hexo 提供了简化的命令：

````sh
hexo g = hexo generate  // 生成
hexo d = hexo deploy    // 部署
hexo s = hexo server    // 运行服务器
````

运行部署命令，成功后会出现以下的信息：
````sh
To https://github.com/rakkang/rakkang.github.io.git
   a796df0..4d4cb79  master -> master
Branch master set up to track remote branch master from https://github.com/rakkang/rakkang.github.io.git.
[info] Deploy done: github
````

部署后，在浏览器地址输入：`USERNAME`.github.io *(eg: [rakkang.github.io](http://rakkang.github.io))* 就可以打开线上的博客了，至此部署完成。

## 写博客、发布文章
新建一篇博客，执行下面的命令：
````sh
$ hexo new post "article title"
````

这条命令会在你的博客目录的 source/\_posts 下生成一个 article-title.md 的文件，用编辑器打开就可以编辑文章了。文章编辑好之后，运行生成、部署命令：
````sh
$ hexo g   // 生成
$ hexo d   // 部署
````
