# 开发优良脚本

## 优良脚本的要素

本指南只要是关于上面的shell构成部分，脚本。在继续之前我们应该考虑一些东西：

1. 一个脚本应该无错运行。
2. 它应该完成他要完成的任务。
3. 程序的逻辑结构定义清晰而且明显。
4. 一个脚本不做不必要的工作。
5. 脚本可以重用。

## 结构

shell脚本的结构非常具有灵活性。即使在Bash中有很大的自由度，你必须保证正确的逻辑，流控制和效率，这样，用户才能执行脚本能做到简单和正确。

当开始一个新的脚本的时候，问自己以下几个问题：

* 需要从用户或者用户环境来取得任何信息吗？
* 怎么样来存放那些信息？
* 需要创建文件吗？哪里和文件需要拥有什么样的权限和所有权？
* 我将用到什么命令？当在一个不同的系统使用脚本的时候，所有这些系统在要求的版本下有这些命令吗？
* 用户需要什么提示吗？什么时候和为什么？

## 术语

下表给出了你需要熟悉的编程条款的概览：

| 术语 | 含义 |
| ------ | ------ |
| 命令控制 | 测试一个命令的退出状态来决定应该执行哪部分的程序。 |
| 条件分支 | 在程序中条件决定接下来该怎么办的逻辑点。 |
| 逻辑流程 | 程序的总体设计。决定任务的逻辑顺序，使得结果是成功的且是可以控制的。 |
| 循环 | 程序执行0次或者多次的部分。 |
| 用户输入 | 程序运行的时候外部源提供的信息，可以在需要的被存储和回调。 |

## 关于顺序和逻辑

为了加速开发的进程，程序的逻辑顺序应该在实现思考好。这是你开发脚本的第一步。

由很多方法可以使用；最常用的就是使用列表。逐条列记和一个程序相关的任务列表允许你描述每个进程。单个任务可以用他们的项目编号来引用。

使用你自己的口语来制定你的程序会执行的任务将帮助你建立易于理解的框架。之后，你可以用shell语言和结构来替换它们。

下面的例子现实了这样一个逻辑流程设计。描述了日志文件的轮换。例子显示了一个可能的重复循环，以你想轮换的基本日志文件的数量来控制：

1. Do you want to rotate logs?
 1. If yes:
  1. Enter directory name containing the logs to be rotated.
  2. Enter base name of the log file.
  3. Enter number of days logs should be kept.
  4. Make settings permanent in user's crontab file.
 2. If no, go to step 3.
2. Do you want to rotate another set of logs?
  1. If yes: repeat step 1.
  2. If no: go to step 3.
3. Exit

用户应该提供信息给程序来运行。必须得到并储存来自用户的输入。用户应该注意到她的crontab会发生改变。

## 一个Bash脚本的例子：mysystem.sh

[`mysystem.sh`](https://github.com/ShadowRZ/Bash-Beginners-Guide-CN/blob/master/Scripts/mysystem.sh) 脚本执行了一些熟悉的命令，(`date`, `w`, `uname`, `uptime`) 来显示你和你机器的信息。

```bash
#!/bin/bash
# mysystem.sh
clear
echo "This is information provided by mysystem.sh.  Program starts now."

echo "Hello, $USER"
echo

echo "Today's date is `date`, this is week `date +"%V"`."
echo

echo "These users are currently connected:"
w | cut -d " " -f 1 - | grep -v USER | sort -u
echo

echo "This is `uname -s` running on a `uname -m` processor."
echo

echo "This is the uptime information:"
uptime
echo

echo "That's all folks!"
```

脚本总是以相同的2个字符开始，“#!”。之后，shell会执行定义在第一行之后的命令。脚本在第1行清除屏幕内容。第2行打印一条语句，通知用户将要发生的事情。第5行问候用户。第6，9，13，16和20行是为了按顺序输出显示。第8行打印了当前的日期和周数。第11行市又一个提示信息，和第3，8，22行一样。第12行格式化 `w` 的输出；第15行显示了操作系统和CPU信息。第19行给出了uptime和load信息。

`echo` 和 `printf` 都是Bash内建命令。第一个总是以状态0退出，且简单地把参数在标准输出打印出来，而后者允许定义一个格式化字符串且在失败后返回一个非零的退出状态。

这是一个相同的使用 `printf` 内建命令的脚本：

```bash
#!/bin/bash
clear
printf "This is information provided by mysystem.sh.  Program starts now."

printf "Hello, $USER.\n\n"

printf "Today's date is `date`, this is week `date +"%V"`.\n\n"

printf "These users are currently connected:\n"
w | cut -d " " -f 1 - | grep -v USER | sort -u
printf "\n"

printf "This is `uname -s` running on a `uname -m` processor.\n\n"

printf "This is the uptime information:\n"
uptime
printf "\n"

printf "That's all folks!\n"
```

依靠插入信息来建立友好的脚本将在 [第 8 章 _编写交互脚本_](../Writing-Interactive-Script/README.md) 详细讲解。

**^^^注意^^^ Bourne Again shell的标准位置**

意味着 `bash` 程序安装在 `/bin`。

**!!!警告!!! 如果stdout不存在**

如果你从cron执行脚本，提供完整的路径名字和重定向输出和错误。既然shell在非交互模式运行，如果你考虑这些任何错误将导致脚本过早的退出。

## init脚本例子

一个init脚本启动在UNIX和Linux机器上的系统服务。通常例子有系统日志守护程序，电源管理守护程序，名字和邮件守护程序。这些脚本，也被称作启动脚本，存储在系统的特定位置，比如 `/etc/rc.d/init.d` 或者 `/etc/init.d`。Init，初始化进程，读取它的配置文件来决定在某些运行等级哪些服务来启动或者停止，比如，为了进行管理任务，系统要尽可能设置成非使用状态，比如从备份中恢复一个危急的文件系统。重启动和关闭的运行等级通常也是配置好的。

在启动一个服务或者停止它的时候需要执行的任务纪录在启动脚本中。配置 `init` 是系统管理员的一项任务，由此服务可以在正确的时刻运行和停止。当面临这样的任务，你需要对你系统的启动和停止程序有着良好的理解。我们因此建议在开始着手你的初始化脚本之前阅读 `init` 和 `inittab` 的man页面。

这里是一个很简单的例子，当起动和关闭你的机器的时候会播放一个声音：

```bash
#!/bin/bash

# This script is for /etc/rc.d/init.d
# Link in rc3.d/S99audio-greeting and rc0.d/K01audio-greeting

case "$1" in
'start')
  cat /usr/share/audio/at_your_service.au > /dev/audio
  ;;
'stop')
  cat /usr/share/audio/oh_no_not_again.au > /dev/audio
  ;;
esac
exit 0
```

`case` 语句经常使用在这类脚本当中，在[第 7.2.5 节 “使用exit语句和if”]()中会解释。
