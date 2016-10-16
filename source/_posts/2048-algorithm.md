title: "基于 Python 的命令行 2048"
date: 2015-04-10 14:04:36
tags: 
- Python
- Algorithm
- 2048
categories: Python
toc: true
---

>2048 是一个基于数字合并的消除类游戏，游戏规则比较简单，但是可玩性很强，操作简单、益智。趁着周末有时间，简单的分析了下 2048 的算法并用 Python 写了一个命令行的实现。

<!-- more -->

### 基本规则

- 规则描述：

> 1. 相邻两个相同的数字可以合并
> 2. 每次移动，每个数字最多合并一次
> 3. 所有格子填满且没有相邻数字可以合并时游戏结束

- 单行合并算法
    + 代码实现：

        ````python
        # 数组向左合并
        # 运行结果：[4, 2, 0, 0]

        a = [0, 2, 2, 2]
        i, t, MAX = 1, 0, len(a) - 1

        while t <= MAX:
            if not a[t]: 
                a[t], a[i], i = a[i], 0, min(i + 1, MAX)
            elif a[i] == a[t]: 
                if i！= t:
                    a[i], a[t]， t = 0, 2 * a[i], t + 1
                i = min(i + 1, MAX)
            elif a[i] != a[t] and a[i]: 
                t = t + 1
            else: 
                i = min(i + 1, MAX)

            i = min(max(t + 1, i), MAX)
            if i == MAX and not a[i]: break       
        ````
    + 算法分析：
        __t__：  表示等待合并的位置
        __i__：  扫描的索引
        1. 如果 __t__ 位置为空，则交换 __i__ 和 __t__ 的值，并且 __i__ 位移加 1
        2. 如果 __i__ 和 __t__ 对应的值相等且有 __t__ != __i__, 则合并 __i__ 和 __t__ 的值，同时 __t__ 和 __i__ 位移加 1
        3. 如果 __i__ 和 __t__ 对应值不相等且 __i__ 对应值不为空，则 __t__ 的位移加 1
        4. 如果 __i__ 到达末尾且对应的值不为空，则等待 __t__ 到达队尾
        5. 如果 __i__ 到达队尾且对应值为空，则表示本次扫描结束
        ````
        运算过程：
        [0, 2, 2, 2] i->1, t->0
        [2, 0, 2, 2] i->2, t->0
        [4, 0, 0, 2] i->3, t->1
        [4, 2, 0, 0] i->3, t->1
        ````

### Python 实现
- 运行截图：

    ![命令行 2048](/img/2048.png)

- 操控：

````python
OPTIONS_KEY = {
            'w': 2,             # up
    'a': 1,         'd': 3,     # left, right
            's': 4,             # down
            'k': 2,             # up
    'h': 1,         'l': 3,     # left, right
            'j': 4              # down
}
````

- 实现代码：
    + 获取键盘按下事件（getch.py）:

    ````python
    #!/usr/bin/python
    # coding:utf-8

    class Getch:
        """Gets a single character from standard input.  Does not echo to the screen."""
        def __init__(self):
            try: self.impl = _GetchWindows()
            except ImportError:  self.impl = _GetchUnix()
        def __call__(self):
            return self.impl()

    class _GetchUnix:
        def __init__(self):
            import tty, sys

        def __call__(self):
            import sys, tty, termios
            fd = sys.stdin.fileno()
            old_settings = termios.tcgetattr(fd)
            try:
                tty.setraw(sys.stdin.fileno(), termios.TCSANOW)
                ch = sys.stdin.read(1)
                sys.stdout.write(ch)
            finally: termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
            return ch

    class _GetchWindows:
        def __init__(self): import msvcrt
        def __call__(self):
            import msvcrt
            return msvcrt.getch()

    if __name__ == '__main__':
        getch = Getch()
        print "Getch TEST：Enter 'q' exit!",
        while True:
            print '\nYour input: ',
            ch = getch()
            if ch == 'q': break
    ````

    + 2048 游戏代码（2048.py）:

        ````python
        #!/usr/bin/python
        # coding:utf-8

        import random

        SPLITE_LINE = ' - - - - - - - - - - - - - - - - -'
        OVER_TIPS   = ' >> GAME OVER << Enter (R)eplay,(Q)uit: '
        INPUT_TIPS  = ' Enter (W)Up,(S)Down,(A)Left,(D)Right: '
        OPTIONS_KEY = {
                    'w': 2,
            'a': 1,         'd': 3,
                    's': 4,

                    'k': 2,
            'h': 1,         'l': 3,
                    'j': 4
        }

        MAX_SIZE  = 4
        GAME_GRID = [
            [0, 0, 0, 0],
            [0, 0, 0, 0],
            [0, 0, 0, 0],
            [0, 0, 0, 0]
        ]

        GRID_BOARD = [
            [0x3, 0x7, 0xa, 0xf],     # right
            [0xc, 0xd, 0xe, 0xf],     # down
            [0x0, 0x4, 0x8, 0xc],     # left
            [0x0, 0x1, 0x2, 0x3]      # up
        ]

        GENERATED_LOC = [-1, -1]


        def print_grid():
            print '\033[0G', SPLITE_LINE
            for row in xrange(MAX_SIZE):
                print ' |',
                for column in xrange(MAX_SIZE):
                    if not GAME_GRID[row][column]: 
                        print '\033[1;37;42m', '     ', '\033[0m',
                    elif [row, column] == GENERATED_LOC: 
                        print '\033[1;37;45m', '\033' + '%5d' % (GAME_GRID[row][column]), '\033[0m',
                    else: 
                        print '\033[1;37;44m', '\033' + '%5d' % (GAME_GRID[row][column]), '\033[0m',
                print '|\n %s' % SPLITE_LINE


        def init():
            for i in xrange(MAX_SIZE ** 2):  GAME_GRID[i / 4][i % 4] = 0
            b, n = [x + 1 for x in range(3)], random.choice([x + 2 for x in range(3)])
            v, l = random.sample([2 ** x for x in b] * 2, n), \
                   random.sample([x for x in range(MAX_SIZE ** 2)], n)
            for x in xrange(n): GAME_GRID[l[x] / 4][l[x] % 4] = v[x]


        def move(ori):
            merged = False
            if ori <= 2: d = 1
            else: d = -1
            for col in xrange(MAX_SIZE):
                if d > 0: i, t, MAX, = 1, 0, MAX_SIZE - 1
                else: i, t, MAX, = -2, -1, - MAX_SIZE
                while d * t <= d * MAX:
                    if (not GAME_GRID[t][col] and not ori % 2) or \
                       (not GAME_GRID[col][t] and ori % 2):
                        if not ori % 2:
                            GAME_GRID[t][col], GAME_GRID[i][col], merged = GAME_GRID[
                                i][col], 0, (merged or GAME_GRID[i][col] != 0)
                        else:
                            GAME_GRID[col][t], GAME_GRID[col][i], merged = GAME_GRID[
                                col][i], 0, (merged or GAME_GRID[col][i] != 0)
                        i = d * min(d * (i + d), d * MAX)
                    elif ((GAME_GRID[i][col] == GAME_GRID[t][col] and not ori % 2) or
                          (GAME_GRID[col][i] == GAME_GRID[col][t] and ori % 2)):
                        if i != t:
                            if not ori % 2: GAME_GRID[i][col], GAME_GRID[t][col] = 0, 2 * GAME_GRID[i][col]
                            else: GAME_GRID[col][i], GAME_GRID[col][t] = 0, 2 * GAME_GRID[col][i]
                            t = t + 1
                        i, merged = d * min(d * (i + d), d * MAX), i != t or merged
                    elif (GAME_GRID[i][col] != GAME_GRID[t][col] and 
                          GAME_GRID[i][col] and not ori % 2) or \
                         (GAME_GRID[col][i] != GAME_GRID[col][t] and 
                          GAME_GRID[col][i] and ori % 2): t = t + d
                    else: i = d * min(d * (i + d), d * MAX)
                    i = d * min(max(d * (t + d), d * i), d * MAX)
                    if i == MAX and ((not GAME_GRID[i][col] and not ori % 2) or
                                     (not GAME_GRID[col][i] and ori % 2)):
                        break

            return merged


        def is_game_over():
            for i in range(MAX_SIZE):
                for j in range(MAX_SIZE):
                    if not GAME_GRID[i][j] or (3 - j and GAME_GRID[i][j] == GAME_GRID[i][j + 1]) or \
                        (3 - i and GAME_GRID[i][j] == GAME_GRID[i + 1][j]): return False
            return True


        def generate_new(ori):
            bounds = GRID_BOARD[ori]
            for i in bounds:
                if not GAME_GRID[i / 4][i % 4]:
                    GAME_GRID[i / 4][i % 4] = random.choice([2, 4, 8])
                    GENERATED_LOC[0] = i / 4
                    GENERATED_LOC[1] = i % 4
                    break


        if __name__ == '__main__':
            import getch

            def clear_screen():
                print '\033[2k\033[1A' * 9,

            def ready():
                init()
                print_grid()
                print INPUT_TIPS,

            game_over = False
            get_input = getch.Getch()
            INPUT_TIPS_LEN = len(INPUT_TIPS)
            GAME_OVER_TIPS_LEN = len(OVER_TIPS)
            tips_len = INPUT_TIPS_LEN

            ready()

            while True:
                GENERATED_LOC = [-1, -1]
                key = get_input()

                if key == 'q': exit(0)
                elif key in OPTIONS_KEY and not game_over:
                    ori = OPTIONS_KEY[key]
                    if move(ori):
                        generate_new(ori - 1)
                        game_over = is_game_over()
                elif key == 'r' and game_over:
                    game_over = False
                    init()

                clear_screen()
                print_grid()

                if game_over: print OVER_TIPS,
                else: print INPUT_TIPS,
        ````