title: "ADB 的 Python 扩展"
date: 2015-03-14 16:51:29
tags: 
- Android
- Python
categories: Python
toc: true
---

由于一直在 Mac 上做 Android 开发，习惯在命令行下使用 adb 命令调试。在开发调试中经常碰到一些问题：

- 多设备连接时，adb 使用很麻烦，每次都要查询设备id然后复制粘贴
- adb logcat 输出的 Tag 的不同级别不能按颜色区分
- adb logcat 按包名过滤
- adb 截屏并保存到指定路径

经常被这些问题搞得很郁闷，所以决定用 Python 扩展下 adb 命令的功能：

<!-- more -->

- 多设备连接时，可以选择
- adb logcat 不同级别按颜色区分
- 按包名过滤
- adb 截屏并保存到指定路径

### 多设备选择
+ 实现方式：
>如果连接了多设备，先查询出所有可用设备并编号，然后列出带编号的设备。选择编号就可以连接`adb`到指定设备：

+ 运行截图：

![multiple_devices_adb](/img/multi_adb.png)

+ 实现脚本：
    - 多设备选择(*libadb.py*)：

    ````python
    #!/usr/bin/python
    # coding:utf-8

    import os
    import re
    import sys

    def select_target_devices():
        devices = os.popen("adb devices -l | sed '1d' | sed '/./!d'").readlines()

        # If only one devices, don't select anything
        if len(devices) > 1:
            remodel = re.compile("^(.*)device.*model:(.*)device.*$")

            def read_a_target(max):
                print 'Target:',

                try:
                    var = raw_input()
                except KeyboardInterrupt:
                    sys.stdout.flush()
                    exit(1)

                try:
                    var = int(var)
                    if 0 <= var <= max:
                        return var
                    return read_a_target(max)
                except Exception, e:
                    return read_a_target(max)

            print 'Select the target device: '
            for i in range(len(devices)):
                res = remodel.match(devices[i])
                if res:
                    id, model = res.groups()
                    print '[%d]: %s -> %s' % (i, id.strip().upper(), model.strip())
                else:
                    tmparr = re.compile("\\s+").split(devices[i])
                    print '[%d]: %s -> %s' % (i, tmparr[0].upper(), tmparr[1])

            target = read_a_target(len(devices) - 1)
            res = remodel.match(devices[target])
            if res:
                id, model = res.groups()
                return id.strip()
            else:
                tmparr = re.compile("\\s+").split(devices[target])
                print 'The target is %s' % tmparr[1]
                exit(0)
        else:
            return None
    ````

    - adk 脚本：

    ````python
    #!/usr/bin/python
    # coding:utf-8

    import os
    import re
    import sys
    import libadb       # 导入自定义的 libadb.py

    args = sys.argv[1:]

    dev = None

    # If no special device and args not null
    if (not '-s' in args) and len(args) and (args[0] != 'devices'):
        dev = libadb.select_target_devices()

    if dev:
        dev = '-s %s' % dev
    else:
        dev = ''

    os.system('adb %s %s' % (dev, ' '.join(args)))
    ````


一般为了方便使用，会更改执行权限并链接到用户的命令目录：

````sh
$ chmod a+x adk.py

$ ln -s ~/python/adb/adk.py /usr/local/bin/adk
# 之后就可以使用 adk 替换 adb 命令的使用了
$ adk logcat   # 相当于 adb logcat
````

### adb logcat 带颜色输出

+ 实现方式：

>之前在 [StackOverflow](http://stackoverflow.com) 上找到了一个实现：[http://stackoverflow.com/questions/11204202/color-of-adb-logcat-in-ubuntu-command-line](http://stackoverflow.com/questions/11204202/color-of-adb-logcat-in-ubuntu-command-line), 但是实现的不太好，有几个问题：

 - 如果输出带时间的 log 彩色效果会失效
 - 不能持续的等待 adb logcat 的输出，经常会突然断开
 - 多设备连接的问题

    >针对以上问题做了改进并去掉了log的格式化效果
    >级别颜色：
    >"V":··········# 灰色
    >"I":··········# 绿色
    >"D":··········# 青色
    >"W":··········# 黄色
    >"E":··········# 红色

+ 效果截图：

![color_adblog](/img/color_adblog.png)

+ 按包名过滤的使用：

````shell
$user@shell ~: adblog -s <Tag1 ...> -app [packages ...]
````

+ 实现脚本(*adblog.py*)：

````python
#!/usr/bin/python
# coding:utf-8

import os
import re
import sys
import libadb       # 导入自定义的 libadb.py

raw_args = sys.argv[1:]

dev = libadb.select_target_devices()

if dev:
    dev = '-s %s' % dev
else:
    dev = ''

def parse_packages():
    if '-app' in raw_args:
        i = raw_args.index('-app')
        sub_arr = raw_args[i:]
        out_args = None
        for arg in sub_arr:
            if arg.startswith('-'):
                if arg == '-app':
                    continue
                else:
                    break
            elif not ':' in arg:
                if not out_args:
                    out_args = []

                out_args.append(arg.replace('*', ''))

        raw_args.remove('-app')
        return out_args
    else:
        return None


def peek_special_package(package):
    return os.popen("adb %s shell ps | grep -E '%s' | cut -c10-15 | xargs"
                    % (dev, package)).readline().strip().split(' ')

packages = parse_packages()

if packages:
    for arg in packages:
        if arg in raw_args:
            raw_args.remove(arg)

    packages = peek_special_package('|'.join(packages))

# to pick up -d or -e
adb_args = ' '.join(raw_args)

if adb_args.startswith('--help'):
    os.system('adb logcat --help')
    print '\033[01;32mFilter by package name:\033[0m                                        '
    print '[option]:                                                                        '
    print '  -app <pacakges>        fuzzy match the packge name, eg: "google" will match all'
    print '                         google apps: com.google, com.android.google and so on.  '
    print '                                                                                 '
    print 'User for zsh: if use "*" eg: adblog *:W, need add prefix "noglob"; Like bellow:  '
    print '                                                                                 '
    print '  $zsh@usr ~: noglob adblog *:W -app com.google com.anroid                       '
    print '                                                                                 '
    print '                                                                                 '
    exit(0)


# if someone is piping in to us, use stdin as input.  if not, invoke adb logcat
if os.isatty(sys.stdin.fileno()):
    print "[exec]: adb %s logcat %s" % (dev, adb_args)
    input = os.popen("adb %s logcat %s" % (dev, adb_args))
else:
    input = sys.stdin

COLORS = {
    "V": "\033[22;37m",         # 灰色
    "I": "\033[22;32m",         # 绿色
    "D": "\033[22;36m",         # 青色
    "W": "\033[22;33m",         # 黄色
    "E": "\033[22;31m",         # 红色
}

retag = re.compile("(.*)([A-Z])/([^\(]+)\(([^\)]+)\): (.*)$")

if adb_args.startswith('-c'):
    exit(0)

while True:
    try:
        line = input.readline()
    except KeyboardInterrupt:
        sys.stdout.flush()
        break

    match = retag.match(line)
    if match:
        pref, tagtype, tag, owner, message = match.groups()

        # filter by multiple package name
        if packages and owner and not owner.strip() in packages:
            continue

        if tagtype in COLORS:
            line = "%s%s" % (COLORS[tagtype], line)

    if len(line) == 0:
        break

    sys.stdout.write(line)
````
 
### ADB 截屏
+ 实现方式：
>利用 `adb shell` 的 `screencap` 命令，先截取屏幕并保存到设备的 /sdcard 目录，之后利用 `adb pull` 上传到电脑的指定目录，同时删除设备 /sdcard 保存的文件

+ 运行截图：

![adbshot命令](/img/adbshot.png)

+ 实现脚本(*adbshot.py*)：

````python
#!/usr/bin/python
# coding:utf-8

import os
import re
import sys
import time
import libadb       # 导入自定义的 libadb.py

args = sys.argv[1:]

dev = libadb.select_target_devices()

if dev:
    dev = '-s %s' % dev
else:
    dev = ''

destfile = None
if len(args):
    destfile = args[0]

filename = time.strftime('%Y%m%d%H%M%S', time.localtime(time.time()))

if not destfile:
    destfile = "~/Pictures/Screenshot/adbshot/screenshot_%s.png" % filename
else:
    path, tfile = os.path.split(destfile)
    if not os.path.exists(path):
        print "Create path: ", path
        os.system('mkdir -p %s' % path)

    if '' == tfile:
        destfile = '%s/screenshot_%s.png' % (path, filename)
    elif not tfile.endswith('png'):
        destfile = "%s.png" % destfile

print "Save device screenshot to ", destfile
os.system('adb %s shell screencap -p /sdcard/screen.png \
    && adb %s pull /sdcard/screen.png %s && adb %s shell rm /sdcard/screen.png'
          % (dev, dev, destfile, dev))
````