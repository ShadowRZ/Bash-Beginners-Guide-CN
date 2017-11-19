# 调试Bash脚本

## 调试整个脚本

当一些事情不能按照计划进行，你需要确定到底是什么导致了脚本运行失败。Bash提供了大量的调试特性。最通常的做法是使用 `-x` 选项来启动子shell，这将让整个脚本在调试模式下进行。每个命令和他附加参数的信息会在执行之前被展开并且送到标准输出打印。

以下是脚本 commented-script1.sh 在调试模式下运行。再次注意在脚本的输出中注释是不可见的。

```bash
bash -x script1.sh
###+ clear
###
###+ echo 'The script starts now.'
###The script starts now.
###+ echo 'Hi, willy!'
###Hi, willy!
###+ echo
###
###+ echo 'I will now fetch you a list of connected users:'
###I will now fetch you a list of connected users:
###+ echo
###
###+ w
###  4:50pm  up 18 days,  6:49,  4 users,  load average: 0.58, 0.62, 0.40
###USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU  WHAT
###root     tty2     -                Sat 2pm  5:36m  0.24s  0.05s  -bash
###willy	 :0       -                Sat 2pm   ?     0.00s   ?     -
###willy	 pts/3    -                Sat 2pm 43:13  36.82s 36.82s  BitchX willy ir
###willy    pts/2    -                Sat 2pm 43:13   0.13s  0.06s  /usr/bin/screen
###+ echo
###
###+ echo 'I'\''m setting two variables now.'
###I'm setting two variables now.
###+ COLOUR=black
###+ VALUE=9
###+ echo 'This is a string: '
###This is a string:
###+ echo 'And this is a number: '
###And this is a number:
###+ echo
###
###+ echo 'I'\''m giving you back your prompt now.'
###I'm giving you back your prompt now.
###+ echo
###
```

**^^^注意^^^ 未来bash的特性**

现在有一个全新的Bash调式工具出现在[SourceForge](http://bashdb.sourceforge.net/)。然而现在，你需要一个已经打过补丁的Bash-2.05。新的特性可能会在bash-3.0中出现。

## 调试部分脚本

使用bash内建命令 `set` 可以让那些确定没有错误的部分以正常模式运行，而只对有错误的部分显示其debug信息。比如我们不确定在 `commented-script1.sh` 里面 `w` 命令会做些什么，那么我们可以把它象这样包含起来： 
```bash
set -x			# activate debugging from here
w
set +x			# stop debugging from here
```

输出看来就像这样：

```bash
script1.sh
###The script starts now.
###Hi, willy!
###
###I will now fetch you a list of connected users:
###
###+ w
###  5:00pm  up 18 days,  7:00,  4 users,  load average: 0.79, 0.39, 0.33
###USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU  WHAT
###root     tty2     -                Sat 2pm  5:47m  0.24s  0.05s  -bash
###willy    :0       -                Sat 2pm   ?     0.00s   ?     -
###willy    pts/3    -                Sat 2pm 54:02  36.88s 36.88s  BitchX willyke
###willy    pts/2    -                Sat 2pm 54:02   0.13s  0.06s  /usr/bin/screen
###+ set +x
###
###I'm setting two variables now.
###This is a string:
###And this is a number:
###
###I'm giving you back your prompt now.
###
```

你可以在同样的脚本里多次打开关闭调试模式。

下表给出了其他有用的bash选项概貌：

### 表 2.1. 设置调试选项概览

| 短符号 | 长符号 | 结果 |
| ----- | ----- | --- |
| `set -f` | `set -o noglob` | 禁止特殊字符用于文件名扩展。 |
| `set -v` | `set -o verbose` | 打印读入shell的输入行。 |
| `set -x` | `set -o xtrace` | 执行命令之前打印命令。 |

横线用来激活一个shell选项，再使用一次将解除。不要搞错了！

以下例子，我们在命令行里面证明这些选项：

```bash
set -v

ls
###ls 
###commented-scripts.sh	   script1.sh

set +v

ls *
###commented-scripts.sh    script1.sh

set -f

ls *
###ls: *: No such file or directory

touch *

ls
###*   commented-scripts.sh    script1.sh

rm *

ls
###commented-scripts.sh    script1.sh
```

或者，这些模式也可以在脚本里面指定，只需在第一行shell的声明中加入需要的选项。选项可以叠加，和通常UNIX命令一样：

```bash
#!/bin/bash -xv
```

每当你找到脚本中的错误部分，你可以在每个你不确定的命令之前增加 `echo` 语句，这样你就会明确的看到哪里或者为什么脚本没有正常工作。在 `commented-script1.sh` 例子中，可以这么做，依然假设显示用户这部分出了问题： 

```bash
echo "debug message: now attempting to start w command"; w
```

在更高级的脚本中， `echo` 可以插入到在不同的阶段显示变量表，因此可以检查到错误： 

```bash
echo "Variable VARNAME is now set to $VARNAME."
```
