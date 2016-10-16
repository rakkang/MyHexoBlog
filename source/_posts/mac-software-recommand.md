title: "Mac 常用软件"
date: 2015-04-07 17:54:04
tags: Software
categories: Software
toc: true
---
### 快速启动神器 [Alfred](http://www.ifunmac.com/2015/02/alfred-2-6/)
>OSX 自带了 Spotlight 工具，在 Mac 下搜索本地文件、软件非常的方便，很多事情不用鼠标，按几下键盘就可以高效的完成。Alfred 可以说是 Mac 上最佳的 Spotlight 替代软件，不仅覆盖了 Spotlight 的全部功能，最大的特点是支持自定义Workflow。

![alfred2](/img/alfred_0.png)

<!-- more -->

相关链接:
- [知乎上的Alfred](http://www.zhihu.com/question/20656680)
- [Github 上的 Workflow](https://github.com/hzlzh/Alfred-Workflows)

### 翻墙利器 [ShadowsocksX](https://github.com/shadowsocks/shadowsocks)
>Mac上得翻墙利器，支持域名智能代理。ShadowsocksX 提供了一个公共的服务器，同时也可以自己在国外搭建代理，非常方便

### 状态栏图标管理 [Bartender](http://www.macbartender.com)
>Bartender 是一款Mac上的菜单栏图标管理工具，能够还你一个干净的Mac菜单栏，它能够创建一个二级菜单，可以把不需要显示在菜单栏上的图标放到二级菜单中或者直接隐藏。

- 使用 Bartender 前的状态栏：

![bartender0](/img/bartender_0.png)

- 使用 Bartener 之后：

![bartender0](/img/bartender_1.png)

### 菜单栏日历工具 [Fantastical](http://www.ifunmac.com/2015/03/fantastical-2/)
>Mac 菜单自带的时间显示太简陋了，只能查看时间。Fantastical 是一个菜单的日历工具，在菜单栏上显示一个带日期的图标，点击之后可以像 Windows 一样可以查看日历、待办事项等，同时 Fantastical 可以同步 Exechange 服务。

- 使用截图：

![Fantastical](/img/caltool.png)

### 开发文档神器 [Dash](https://kapeli.com/dash)
>Dash 是一款 Mac 上的API文档管理和代码片段收藏工具，Dash内置了丰富的API文档，让我们集中管理API文档，包括下载、搜索、查阅，包括各种主流的编程语言和框架，如Cocos2D, Cocos3D, Corona, CSS, HTML, Java, Objective-C, JavaScript, jQuery, Kobold2D, Lua, 不需要我们再去到处下载 API 文档，Dash 已经自动集成了，最新的2.2版本支持OS X Yosemite，Swift、iOS 8等，并支持集成到XCode、Alfred等软件中，非常的强大，Mac 上的开发者必备的 API 文档管理工具。

- 使用截图：

![Dash](/img/Dash.png)

### 终端替代工具 [iTerm2](http://iterm2.com)
>Mac 对原生 Shell 的支持是无数程序员喜爱 Mac 的理由之一，程序员用 Mac 而不用 Shell，基本等于自断一臂，威力将大打折扣。Shell 并非凭空而来，它的入口是终端工具。OS X自带的终端工具虽然不错，但是和 iTerm 2一比，就逊色很多了。

- 一些基本功能如下：

    1. 分窗口操作：shift+command+d（横向）command+d（竖向）
    2. 查找和粘贴：command+f，呼出查找功能，tab 键选中找到的文本，option+enter 粘贴
    3. 自动完成：command+; 根据上下文呼出自动完成窗口，上下键选择
    4. 粘贴历史：shift+command+h5、回放功能：option+command+b
    6. 全屏：command+enter
    7. 光标去哪了？command+/
    8. Expose Tabs：Option+Command+E

- 使用截图：

![iTerm2](/img/iterm2.png)

### 终极 Shell [oh my zsh](https://github.com/robbyrussell/oh-my-zsh)
>Mac 虽然自带了 Bash，但是功能太简陋了，只能算是阉割版的 Shell。Zsh 是类 Unix 系统通用的 Shell，功能强大到 ”令人发指“ 的地步，各种 Themes、Plugins 用之不尽，对 Git 项目有很好的支持

- 使用截图：

![oh-my-zsh](/img/zsh.png)

- 配置：
    - zsh 是有自己的环境变量配置文件，一般放在 ~/.zshrc
    - 安装完 zsh 后，会在 $HOME 的根目录生成 ..oh-my-zsh/ 目录，是 zsh 配置文件以及内置脚本的根目录

相关链接：
- [官网](http://ohmyz.sh)
- [知乎专栏](http://zhuanlan.zhihu.com/mactalk/19556676)
- [Zsh 配置](http://blog.chinaunix.net/uid-26495963-id-3193686.html)

### 包管理 [Homebrew](https://github.com/Homebrew/homebrew)
>Mac OS X是基于Unix的操作系统，可以安装大部分为Unix/Linux开发的软件。然而，如果只是以使用为目的，对每个软件都进行手工编译不是很方便，也不利于管理已安装的软件，于是出现了类似于Linux中APT、Yum等类似的软件包管理系统，其中最著名的有MacPorts、Fink、Homebrew等。与其他类似工具相比，Homebrew的原则优势是：它尽可能地利用系统自带的各种库，使得软件包的编译时间大为缩短；同时由于几乎不会造成冗余，软件包的管理也清晰、灵活了许多。Homebrew的另一个特点是使用Ruby定义软件包安装配置（叫做formula），定制非常简单

- 使用截图：

![Homebrew](/img/brew.png)

- 常用命令：

    ````sh
    $ brew search wget        # 查找软件包
    $ brew install wget       # 安装软件
    $ brew list               # 列出已安装软件包
    $ brew remove wget        # 删除软件包
    $ brew info wget          # 查看已安装包信息
    $ brew deps wget          # 列出软件包的依赖包
    $ brew update             # 更新 homebrew
    $ brew outdated           # 列出需要更新的软件包
    $ brew upgrade            # 更新过时的软件包
    ````
