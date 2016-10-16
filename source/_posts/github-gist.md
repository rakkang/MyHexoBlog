title: Github·Gist使用攻略
date: 2014-08-30 16:23:07
tags: 
- Github
- Gist
categories: Software
toc: true
---
[Gist](https://gist.github.com) 是 [Github](https://github.com) 推出的基于 Git 的代码片段管理服务。用户可以提交自己的代码片段或任意的文本，可以作为个人的代码管理库、文档管理库等。同时 [Gist](https://gist.github.com) 页面提供访问的 [JavaScript](http://www.baidu.com/s?wd=javascript) 片段用于嵌入其他网站，如：个人博客等。
>但是很多站点粘贴 JavaScript 无效，这时候你可以在 Gist URL 后附加.pibb，得到一个纯 HTML 的版本，然后就可以复制粘贴 HTML 源码到其他网站了。例如: [https://gist.github.com/tiimgreen/10545817.pibb](https://gist.github.com/tiimgreen/10545817.pibb)

<!-- more -->

[Gist](https://gist.github.com) 作为程序猿居家旅行必备的代码管理工具，极大的方便了 Coder。下面是自己的一些使用 [Gist](http://gist.github.com) 经验，分享给各位看官：

使用 [Gist](https://gist.github.com) 前提必须注册 [Github](http://github.com) 账户，账户注册比较简单，这里就不做介绍了
>如果不知 Github、Git为何物的童鞋就当这个页面不存在吧 ^\_^.  

使用、管理 [Gist](https://gist.github.com) 的几种方式：

## 使用 [Github Gist](https://gist.github.com)

>Gist 是以问件的方式管理的，每条 Gist 

可以包含若干个文件，文件内容会自动根据选择存储的类型高亮代码，除了支持各种的编程语言外还支持一些常见的文本，如：Markdown、Diff、TeX等。
登录 [Github](https://github.com) 后访问 [https://gist.github.com](https://gist.github.com) 就打开了个人的 Gist 管理界面，如图点击右上角的 “+” 就可以新建一个 Gist了：

>![创建gist](/img/new_tips.png)
    
[Gist](https://gist.github.com) 的整体界面相当的简洁、明了，操作也比较简单。如果不想这条 Gist 被搜索引擎抓取，可以选择创建私有的 Gist, 但是还是可以通过 URL直接访问到的。

>注意：Gist 的私有和 Github 的私有项目是不同的
>
>![私有Gist](/img/private.png)

## 使用 [Sublime Text](http://www.baidu.com/s?wd=sublime text) + [Gist插件](https://github.com/condemil/Gist)

>Sublime Text 是一个代码编辑器（Sublime Text 2是收费软件，但可以无限期试用），也是HTML和散文先进的文本编辑器。Sublime Text是由程序员Jon Skinner于2008年1月份所开发出来，它最初被设计为一个具有丰富扩展功能的Vim。--- 百度百科

> 不知 Sublime Text 为何物或者无爱的请忽略
    
安装 Gist 插件：
    
打开 Sublime Text, 按下快捷键`CTRL(⌘)+SHIFT+P`打开 Package Control面板，输入`Install packages`在弹出的插件列表中搜索`Gist`,回车就开始安装 [Gist插件](https://github.com/condemil/Gist) 了
如果没有安装 Package Controll 插件，请先安装, 打开 Sublime Text的_Console_ (`CTRL+反引号`) 输入以下代码：
   
   
 - _Sublime Text 3_

   import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())

 - _Sublime Text 2_

   import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')

 - _手动安装_

   可能由于各种原因，无法使用代码安装，那可以通过以下步骤手动安装Package Control：  
   1. 点击Preferences > Browse Packages菜单  
   2. 进入打开的目录的上层目录，然后再进入Installed Packages/目录  
   3. 下载Package Control.sublime-package并复制到Installed Packages/目录  
   4. 重启Sublime Text。

安装 Gist 之后，配置 Github 的账户 Token，用命令行工具输入以下命令：
````shell    
curl -v -u USERNAME -X POST https://api.github.com/authorizations --data "{\"scopes\":[\"gist\"], \"note\": \"SublimeText 2/3 Gist plugin\"}"
````

其中 `USERNAME` 你的 Github 帐号名。执行成功之后会输出以下代码：
````json
{
    "id": 107···41,
    "url": "https://api.github.com/authorizations/107···41",    "app": {
        "name": "SublimeText 2/3 Gist plugin (API)",
        "url": "https://developer.github.com/v3/oauth_authorizations/",
        "client_id": "00000000000000000000"
    },
    "token": "a28b686c3a74······c300f42414c7",
    "note": "SublimeText 2/3 Gist plugin",
    "note_url": null,
    "created_at": "2014-08-25T10:54:00Z",
    "updated_at": "2014-08-25T10:54:00Z",
    "scopes": [
        "gist"
    ]
}
````
    
按下图所示打开 Gist 插件的配置文件：

![menu](/img/menu.png)

添加 Github Token:
````json
{
    // Your GitHub API token
    // see: https://github.com/condemil/Gist#generating-access-token
    "token": "a28b686c3a74······c300f42414c7", // 从命令行得到的 token
}
````

以上，Sublime Text 2/3 的 Gist 插件就配置好了。配置好后，在 Sublime Text 的 `Tools` ⟶ `Gist` 菜单下有使用 Gist 的一些命令，如图：
    
>![gist_tools](/img/gist_tools.png)
    
>使用 Gist 插件很爽的一点是，当你编辑完 gist 文件保存时会自动的帮你同步到 Github·Gist 不用担心没保存丢失，保存 Gist 时可以从 _Console_ 看到同步日志
    
## 使用 [GistBox](http://www.gistboxapp.com/) 服务
>GistBox 提供一种漂亮的方式来组织代码片段。将你的库保存到云端进行备份，再也不用担心丢失。GistBox采用标准的HTML5技术构建。GistBox使用GitHub的后端，但增加了自己的标签和搜索功能层。使用Github账号登陆Gistbox可以将你的代码直接同步进来，反过来，你在GB上的所有改动也都会同步到Github上；GistBox的结构设 计清晰，从左至右分别是主导航（新建Gist，Gists入口，收藏入口-Labels）、Gists列表（Public/Private）、具体代码 区，亲们可以用Label给代码加上各种分辨标签，方便分类整理，在检索代码时可以用顶部的搜索栏，输入关键词或Label可以更快的搜索到目标代码。
    
GistBox 是可以直接用 Github 账户登录的，实时同步你的 Gist 到 Github，很方便。GitBox 从体验上来说更加方便，界面元素丰富，功能也比较全。如果不想用 Sublime Text, 又觉着 Github 自己的 Gist 管理太简单，GistBox是一种不错的方式。
    
>GistBox 同时提供了 Chrome 插件

以上就是我使用 Gist 的一些经验，感谢读完全文！
