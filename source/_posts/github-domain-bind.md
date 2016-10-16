---
title: Github Pages 域名绑定
date: 2016-10-13 22:50:22
tags: Github
categories: Github
toc: true
---
Github 的 Pages 功能不但提供了用户免费搭建博客的功能，同时还提供了自定义的域名绑定；在 Github 上搭建了博客后，Github 会自动给每个网址分配一个二级域名，如：rakkang.github.io，二级域名的问题是太长，也不太容易记住。所以很多用户都会自己申请独立域名并与 Github 的博客绑定起来。

## 域名注册

要绑定独立的域名，首先得注册自己域名，域名一般是收费的；如果网站服务器是在国内的话，域名还需要通过工信部的备案才能使用。推荐使用 [DNSPod](http://dnspod.cn) 注册域名，因为解析是免费的。

<!-- more -->

## 域名绑定

在 DNSPod 上注册好域名之后，要通过设置绑定到指定的服务器在正常访问。如图：

![DNSPod 域名设置](/img/dnspod.png)

Github 提供了两种域名绑定的方式：
>NS 记录是 DNSPod 默认的绑定，不用管

- `A` 记录类型
  `A` 记录绑定是服务器IP地址，一般是如果是顶级域名，如：`www.xxx.com` 等，绑定全站使用这种类型

- `CNAME` 记录类型
  `CNAME` 记录类型绑定是一个域名，一般如果使用的是其他服务器的二级域名时使用

>主机记录类型，默认是`@`，那么访问只能通过 domain.com 访问；
>如果是`wwww`类，那么就可以通过 www.domain.com 访问了

因为 Github 本身提供了一个可访问的二级域名，所以采用 `CNAME` 记录更方便些。

在 DNSPod 设置好绑定的服务器后，要是 Github 帮你解析，还需要在项目的根目录新建一个`CNAME`的文件：

![Github Pages CNAME 文件](/img/github_blog.png)

文件内容很简单，比如申请的域名是：domain.com, 那么填写：
````
domain.com
````
设置好之后，打开项目的 `Settings`, 在 `GitHub Pages` 一栏出现下面的内容说明绑定成功了：

![](/img/github_domain.png)

>在 DNSPod 设置之后，由于DNS缓存的原因，域名解析生效需要一段时间，一般10几分钟就行了。如果在 Github 绑定域名后，出现 404 错误可能是缓存的原因，多试几次或者过一会儿再刷新

如果要测试域名是否解析成功以及具体的解析地址，可以在终端执行`dig`命令查看：
````sh
$ dig ruikye.com +nostats +nocomments +nocmd
````
命令的结果：
````sh
$ dig ruikye.com +nostats +nocomments +nocmd

; <<>> DiG 9.8.3-P1 <<>> ruikye.com +nostats +nocomments +nocmd
;; global options: +cmd
;ruikye.com.                 IN  A
ruikye.com.             600  IN  CNAME   rakkang.github.com.
rakkang.github.com.     30   IN  CNAME   github.map.fastly.net.
github.map.fastly.net.  1    IN  A       103.245.222.133
````
如果结果和在 DNSPod 设置的一致，说明解析以生效了。
