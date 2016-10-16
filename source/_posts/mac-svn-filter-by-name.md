title: OS X/Linux 下过滤 svn log
date: 2015-03-06 13:21:01
tags: 
- Linux
- Python
- svn
categories: Python
toc: true
---
由于经常在 Mac 下做软件开发且个人偏向在命令行下使用 [Subversion(SVN)](https://subversion.apache.org/)。在多人协作开发过程中可能经常需要查看 SVN 的提交历史，所以 `svn log` 这个命令用的比较多，但是在使用过程中发现这个命令还是满足不了自己的一些需求，比如：只显示某一个的提交或者只显示某天的提交等；各种 Google 搜索后还是找不到方法，所以自己就着么用 Python 写了一个按用户和日期过滤 `svn log` 的脚本。

<!-- more -->

```python
#!/usr/bin/python
# coding:utf-8

import sys

argv_len = len(sys.argv)

def help():
    print 'Filter svnlog by user or date!       '
    print 'USEAGE: svnlog [ARGs]                '
    print 'ARGs:                                '
    print '    -n[=name]:                       '
    print '      filter by the special [=name]\n'
    print '    -t[=date]:                       '
    print '      filter by the special [=date]  '
    print 'EXP:                                 '
    print '1. Filter kang\'s commit log       \n'
    print '     svn log -l 50 | svnlog -n=kang\n'


if not argv_len - 1:
    help()
    quit()

author = ''
date = ''

for index in range(1, argv_len):
    argv = sys.argv[index]
    if argv.startswith('-n='):
        author = argv.replace('-n=', '')
    elif argv.startswith('-t='):
        date = argv.replace('-t=', '')
    else:
        help()
        quit()

if author == '' and date == '':
    help()
    quit()

# svn log 输出的分割线
SPLIT_LINE = '------------------------------------------------------------------------'

src = ''.join(sys.stdin.readlines())
lines = src.split(SPLIT_LINE)

for line in lines:
    if author in line and date in line:
        print SPLIT_LINE, line,

if len(lines):
    print SPLIT_LINE
```

为了更加方便的使用，修改了下脚本的权限并且加到用户的命令目录：
````shell
$ mv svnlog.py svnlog          # 重命名使之看起来和系统命令一样

$ chmod a+x svnlog             # 添加执行权限

$ cd /usr/local/bin
$ ln -s ~/mycmd/svnlog filter  # 把执行脚本链接到用户命令目录方便直接使用
````

做完上面几步之后，就可以在命令行中方便的使用了：
````shell
$ svn log | filter -n=ruikye -t=2015-03-04 # 查看 ruikye 在 2015/3/4 日的所有提交
````
