# 执行命令

## 概要

Bash会确定执行的程序的类型。普通程序是那些已经为你的系统编译好的系统命令。当这样的程序 运行的时候，Bash创建一个自身的拷贝因此一个新的进程就被创建。子进程和父进程拥有一样的环境，唯一个区别只有进程号。这个过程被称作内核空间创建进程 forking。

在内核空间创建进程后，子进程的地址空间被新进程数据覆盖。此步骤通过 `exec` 系统调用完成。

_fork-and-exec_ 机制因此把旧的命令转化成新的，虽然新程序的执行环境还是和原来相同，包括输入输出设备的配置，环境变量和优先级。这种机制用来创建所有的UNIX进程，因此对Linux操作系统也起作用，甚至是第一个进程,进程号是1的 `init`，也是在称为 bootstrapping 的启动过程中被fork的。

## Shell内建命令

内建的命令包含在shell本身里面。当内建的命令的名字被用作一个简单命令的第一个词时，shell 直接执行那个命令，而不创建新的进程。内建命令在实现那些单独的程序不可能或者不便实现的某些功能时时很有必要的

bash支持3种内建的命令：

* Bourne Shell内建命令：

    `:,` `.`, `break`, `cd`, `continue`, `eval`, `exec`, `exit`, `export`, `getopts`, `hash`, `pwd`, `readonly`, `return`, `set`, `shift`, `test`, `[`, `times`, `trap`, `umask` and `unset`.

* Bash内建命令：
    `alias`, `bind`, `builtin`, `command`, `declare`, `echo`, `enable`, `help`, `let`, `local`, `logout`, `printf`, `read`, `shopt`, `type`, `typeset`, `ulimit` and `unalias`.

* 特殊内建命令：

    当Bash在POSIX模式运行，特殊内建命令和其他内建命令有3个方面的区别：

    1. 特殊内建命令在shell函数在命令查找期间先被找到。

    2. 如果一个特殊内建命令返回一个错误状态，一个非交互shell就会退出。

    3. 在命令完成后在该命令之前的赋值语句仍然在shell环境中起作用。

    POSIX特殊内建命令是：`:`, `.`, `break`, `continue`, `eval`, `exec`, `exit`, `export`, `readonly`, `return`, `set`, `shift`, `trap` and `unset`.

多数这些内建命令将在下面的章节讨论。关于那些不是这些情况的命令，我们在INFO页面查阅。

## 从脚本执行程序

当程序被一个脚本来执行，bash会使用 `fork` 创建一个新的bash进程。这个子shell一次从shell脚本中读取一行。每行的命令如果直接来自键盘的话会被读取，解释和执行

当子shell处理脚本中的每一行时，父进程等待子进程结束。当运行完shell脚本中每一行时，子shell就结束。父shell苏醒并且显示一个新的提示符。
