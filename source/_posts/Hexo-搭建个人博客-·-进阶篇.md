title: Hexo 搭建个人博客 · 进阶篇
date: 2014-08-30 13:07:26
tags: Hexo
categories: Hexo
toc: true
---
## Hexo 配置文件 *\_config.yml*
Hexo 的各种通用的配置都是在博客根目录行下的 *\_config.yml* 文件中设置的。下面介绍一些常用的配置项：

<!-- more -->

````sh
# Site 基本信息
title: Ruikye                      # 博客标题，如左上角显示
subtitle: ruikye 的个人博客         # 博客副标题
description: 移动开发技术分享博客     # 用于搜索引擎搜索到的描述信息
author: 零雨の夜                    # 博客署名，一般会现在在博客的最下方，rg: &copy;2014 零雨の夜
email: xxx@xxx.com                # 可不填
language: zh-CN                   # 让博客支持中文

...

# Writing

...

highlight:              # 代码高亮
  enable: true          # 开启代码高亮
  line_number: false    # 是否显示行号
  tab_replace: true     # 是否替换 tab 为空格

...

# Pagination
## Set per_page to 0 to disable pagination
per_page: 1             # 文章分页时，每页最多显示文章数，eg: 我的博客在首页和归档页最多只显示一篇文章
pagination_dir: page    # 分页目录

...

# Extensions
## Plugins: https://github.com/hexojs/hexo/wiki/Plugins
## Themes: https://github.com/hexojs/hexo/wiki/Themes
theme: bs-light         # 这里配置博客的主题风格，主题安装在 themes/ 目录下，这里的值就是主题的文件夹名字 
exclude_generator:

plugins:
- hexo-generator-feed   # 安装、启用的插件，这里是启动 RSS 订阅的插件

# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: github                                                  # 博客托管服务器类型
  repository: https://github.com/rakkang/rakkang.github.io.git  # 托管服务器地址
  brach: master                                                 # 博客使用的代码分支
````

除了 Hexo 的通用配置外，每个主题还有各自的配置文件，主题的配置文件放在：themes/[xxx]/\_config.yml, eg: themes/bs-light/\_config.yml，下面以 `bs-light` 为例：

````shell
# 导航栏，如右上角的显示，Tips: RSS 栏是插件添加的不再这里
menu:
  首页: /                 # 格式是：[显示标签]:[索引目录]
  存档: /archives

# 文章右边的小部件
widgets:
# search/tag/category/recent_posts/tagcloud   ----> 这里是 bs-light 的可用小部件
- search                 # 搜索框
- recent_posts           # 最近发布的文章
- category               # 存档目录
- tagcloud               # 文章的标签集合

# 如果在文章的 *.md 中使用 <!-- more -->，那么之后的内容不会在首页显示，而是显示 阅读全文 的链接，显示可以更改
# 如：更多，查看原文等
excerpt_link: 阅读全文

# 博客的社交分享，eg: 博客底部的两个图标
social:
# key weibo/twitter/google/github/stackoverflow/rss
# value url
# e.g github: https://github.com/DaiXiang
  github: https://github.com/rakkang
  rss: /atom.xml

...

cnzz_analytics: true     # 博客的访问统计，这里使用 CNZZ 的统计
# google_analytics:
# rss:

# comment_provider:      # 评论功能，一般使用国内的 多说评论
# Facebook comment
````

## 为文章添加 *Tag*、*Category* 属性
通常，使用下面命令新建的文章模版时，会在文章模版的头部出现一些属性信息：
````
$ hexo new post "a new article"
````
文章模版的属性信息：
````sh
title: Hexo 搭建个人博客 · 进阶篇    # 文章显示标题
date: 2014-08-30 13:07:26         # 文章创建日期
tags: hexo                        # Tag 属性，如果多个 Tag 使用：[Tag 1, Tag 2 ...]
categories: hexo                  # Category 属性，多个时使用：[Category 1, ...]；默认是没有这个的，下面有介绍
---
````
在模版中添加 *Categories* 属性，在博客根目录下 scaffolds/ 文件夹中有各种文章类型的模版，比如在：scaffolds/post.md 中：
````sh
title: title
date: date
tags:
categories:                       # 在这里添加 Category 属性，如果要修改其他的模版，同样的修改方式
````

## 添加多说评论
Hexo 默认使用的 Facebook 的评论代码，在天朝由于众所周知的原因，不能使用。这里介绍使用过年的多说评论：
多说官网：[http://duoshuo.com/](http://duoshuo.com/), 现在多少网址注册，按流程拿到代码：
````
<!-- 多说评论框 start -->
<div class="ds-thread" 
     data-thread-key="请将此处替换成文章在你的站点中的ID" 
     data-title="请替换成文章的标题" 
     data-url="请替换成文章的网址"></div>
<!-- 多说评论框 end -->

<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"ruikye"};
    (function() {
        var ds = document.createElement('script');
        ds.type = 'text/javascript';ds.async = true;
        ds.src = (document.location.protocol == 'https:' ? 
                    'https:' : 'http:') + '//static.duoshuo.com/embed.js';
        ds.charset = 'UTF-8';
        (document.getElementsByTagName('head')[0] 
         || document.getElementsByTagName('body')[0]).appendChild(ds);
    })();
</script>
<!-- 多说公共JS代码 end -->
````
首先，打开 themes/[xxx]/layout/\_partial/article.ejs 文件，在最后面添加多说评论框：
````
<% if (page.comments){ %>
    <!-- 多说评论框 start -->
    ...
    <!-- 多说评论框 end -->
<% } %>
````
然后，打开 themes/[xxx]/layout/\_partial/after_footer.ejs 文件，在最后添加多说 JS 脚本代码：
````
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
...
<!-- 多说公共JS代码 end -->
````
这样多说评论就添加好了，点击文章的标题，在打开的页面下方就会出现多说的评论框。

## 添加百度分享
同样，使用百度分享要先到百度分享官网：[http://share.baidu.com/](http://share.baidu.com/) 按照流程获取 JS 脚本代码。打开 themes/[xxx]/layout/_partial/post/share.ejs，删除文件内容，用获取到的百度分享代码替换：
````
<div class="bdsharebuttonbox">
    <a href="#" class="bds_more" data-cmd="more">
    </a>
    <a href="#" class="bds_qzone" data-cmd="qzone" title="分享到QQ空间">
    </a>
    <a href="#" class="bds_tsina" data-cmd="tsina" title="分享到新浪微博">
    </a>
    <a href="#" class="bds_tqq" data-cmd="tqq" title="分享到腾讯微博">
    </a>
    <a href="#" class="bds_renren" data-cmd="renren" title="分享到人人网">
    </a>
    <a href="#" class="bds_weixin" data-cmd="weixin" title="分享到微信">
    </a>
</div>
<script>
    window._bd_share_config = {
        "common": {
            "bdSnsKey": {},
            "bdText": "",
            "bdMini": "1",
            "bdMiniList": false,
            "bdPic": "",
            "bdStyle": "1",
            "bdSize": "16"
        },
        "share": {},
        "image": {
            "viewList": ["qzone", "tsina", "tqq", "renren", "weixin"],
            "viewText": "分享到：",
            "viewSize": "16"
        }
    };
    with(document) 0[(getElementsByTagName('head')[0] || body).appendChild(createElement('script')).src = 
            'http://bdimg.share.baidu.com/static/api/js/share.js?v=89860593.js?cdnversion=' 
                    + ~ ( - new Date() / 36e5)];
</script>
````
## 安装 RSS 插件
RSS 插件要依赖 Node.js 安装，进入博客的本目了，执行命令：
````shell
$ npm install hexo-generator-feed --save
````
安装之后，可以在通用的 *\_config.yml* 文件中配置插件：
````sh
feed:
    type: atom
    path: atom.xml
    limit: 20
````
## 安装 Sitemap
Sitemap 是搜索引擎抓取网站要用到的，安装命令：
````shell
$ npm install hexo-generator-sitemap --save
````
可选配置：
````sh
sitemap:
    path: sitemap.xml
````
按上面的配置之后，你的博客立马变得功能丰富了很多有木有～～～

## 参考
【1】：https://github.com/hexojs/hexo/wiki/Plugins