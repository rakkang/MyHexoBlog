title: Sublime Text 推荐插件
date: 2014-08-30 16:14:20
tags: Sublime Text
categories: Software
toc: true
---
>Sublime Text 是一个代码编辑器（Sublime Text 2是收费软件，但可以无限期试用），也是HTML和散文先进的文本编辑器。Sublime Text是由程序员Jon Skinner于2008年1月份所开发出来，它最初被设计为一个具有丰富扩展功能的Vim。--- 百度百科

#### [Package Control](https://github.com/wbond/sublime_package_control)
Package Control 是 Sublime Text 管理其他插件的工具，可以实现在线安装、删除插件等功能。

<!-- more -->

安装方法，打开 Sublime Text，按快捷键 `CTRL+反引号` 调出控制台(_Console_), 输入以下代码 ：

    Sublime Text 2 安装代码：
>import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')

    Sublime Text 3 安装代码：
>import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())

    手动安装

>可能由于各种原因，无法使用代码安装，那可以通过以下步骤手动安装Package Control：

>1. 点击Preferences > Browse Packages菜单
 2. 进入打开的目录的上层目录，然后再进入Installed Packages/目录
 3. 下载Package Control.sublime-package并复制到Installed Packages/目录
 4. 重启Sublime Text。
 
 安装完成后，可以使用`CTRL(CMD)+SHIFT+P`快捷键打开控制面板，输入: _package control_ 。

#### [SideBarEnhancements](https://github.com/titoBouzout/SideBarEnhancements)
直接使用 Package Control 安装，SideBarEnhancements是 Sublime Text中强大的 Sidebar 插件，Sublime Text 本身的 Sidebar 提供的右键菜单非常简陋，有 SideBarEnhancements 之后，可以实现很多使用的功能，比如：在文件浏览器中打开文件等。

#### [ConvertToUTF8](https://github.com/seanliang/ConvertToUTF8)
直接使用 Package Control 安装。Sublime Text 本对中文支持不太好，如果直接打开 GBK 编码格式的文件会出现乱码的情况，ConvertToUTF8 可以将 GBK 等其他的编码文件转换为 UTF8 显示，解决中文乱码问题。
>通过本插件，您可以编辑并保存目前编码不被 Sublime Text 支持的文件，特别是中日韩用户使用的 GB2312，GBK，BIG5，EUC-KR，EUC-JP 等。ConvertToUTF8 同时支持 Sublime Text 2 和 3。 ----- ConvertToUTF8 官方介绍

#### [JsFormat](https://github.com/jdc0589/JsFormat)
直接使用 Package Control 安装。JsFormat 顾名思义，是一个可以格式化 JavaScript 代码的插件（也可以格式化Json），格式化是文件名必须保存问 \*.js 或 \*.json, 这样插件才能识别出代码。

#### [Markdown Preview](https://github.com/revolunet/sublimetext-markdown-preview)
直接使用 Package Control 安装。Markdown Preview 是一个支持 Markdown 标记语言的插件，Sublime Text 本身是支持 Markdown 的，但是显示不太美观。同时， Markdown Preview 还支持导出 HTML 或者在浏览器中打开预览。

使用方法：
>打开控制面板（`CTRL+SHIFT+P`），输入: _markdown preview_ 就可看到相关的命令了。

#### [MarkdownEditing](https://github.com/SublimeText-Markdown/MarkdownEditing)
直接使用 Package Control 安装。MardownEditing 是一款功能更加强大的 Markdown 插件。最大的特色是支持 [GFM](https://help.github.com/articles/github-flavored-markdown) ( GitHub flavored Markdown) 代码高亮。
>Markdown plugin for Sublime Text. Provides a decent Markdown color scheme (light and dark) with more robust syntax highlighting and useful Markdown editing features for Sublime Text. 3 flavors are supported: Standard Markdown, GitHub flavored Markdown, MultiMarkdown. 

#### [Spacegray](https://github.com/kkga/spacegray)
直接使用 Package Control 安装。Spacegray 是一款高大上的 Sublime Text 主题。有 Ocean Dark、 Ocean Light 和 Eighties Dark 三中主题。

其他的插件还有：[Tag](https://github.com/SublimeText/Tag), [Gist](https://github.com/condemil/Gist), [Alignment](https://github.com/wbond/sublime_alignment), [All AutoComplete](https://github.com/alienhard/SublimeAllAutocomplete) 等等...