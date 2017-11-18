# Bourne Again SHell的优势

## Bash是GNU shell

GNU 计划(GNU's Not UNIX)为类UNIX系统管理提供遵守UNIX标准的免费软件。

Bash是兼容`sh`的shell且从Korn shell (`ksh`) 和 C shell (`csh`)整合了一些有用的特性。它遵循IEEE POSIX P1003.2/ISO 9945.2 Shell和工具标准。提供了基于sh的编程和交互的功能改进；其中包括命令行编辑，无限制的历史命令，作业控制，shell函数和别名，无大小限制的索引数组，和以2到64为基础的整数算法。Bash可以不经修改地运行多数`sh`脚本。

和其他的GNU项目一样，Bash主动开始保留，保护和促进使用，学习，拷贝，修改和再发布软件的自由。普遍认为这样的情况激发了创造力。这也是Bash程序可以而许多其他shell无法提供的额外特性的缘由。

## Bash独有的特性

### Invocation

除了单字符shell命令行选项可以通常使用内建命令`set`来配置外，你还可以使用几种多字符选项。我们会在本章和后继章节中一系列的更流行的选项中留下深刻印象；完整的列表可以在Bash的info页面中找到，Bash 特性 → 调用 Bash。 

### Bash 启动文件

启动文件是当Bash启动时候读取并且执行的脚本。下面的子章节部分描述了启动shell的不同方法，和因此启动读取的文件。

#### 以交互登陆shell调用，或者使用'--login'

交互意味着你可以输入命令。Shell没有运行因为一个脚本被激活了。一个登陆shell就是在系统验证完了你输入的用户名和密码后得到shell。

读取的文件：

* `/etc/profile`

* `~/.bash_profile`, `~/.bash_login` or `~/.profile`： 读取第一个存在的可读取的文件

* `~/.bash_logout` 登出的时候。

错误消息将会显示如果配置文件存在但是不能读取。一个文件不存在，Bash将搜索下一个。

#### 以一个交互非登陆shell调用

一个非登陆shell就是不需要进行系统的认证。比如，通过一个图标打开一个终端，或者一个菜单项目，那样就是非登陆shell

读取的文件：

* `~/.bashrc`

此文件通常指向 `~/.bash_profile`：

```bash
if [ -f ~/.bashrc ]; then . ~/.bashrc; fi
```

参见 [第 7 章 条件语句](../Conditional-Statements/README.md) `if` 结构以得到更多信息。

#### 非交互调用

所有脚本使用非交互shell。他们是被编制出来完成特定任务且不能完成其他工作。

读取的文件：

* 由变量 `BASH_ENV` 定义

PATH 不是用来搜索文件的，所以如果你想使用它，最好给出全部路径名和文件名来。

#### 以sh命令调用

Bash尝试 sh 的相似行为同时也遵循POSIX标准。

读取的文件：

* `/etc/profile`

* `~/.profile`

当以交互方式调用时，环境变量 `ENV` 能指出额外的启动信息

#### POSIX模式

本选项在使用内建的命令：`set`

```bash
set -o posix
```

或者调用 `bash` 程序附带 `--posix` 选项。Bash会试着尝试尽可能遵循POSIX的shell标准。 设置 `POSIXLY_CORRECT` 变量可以达到目的。

读取的文件：

* 由变量 `ENV` 定义。

#### 远程调用

以 rshd调用时读取的文件：

* ~/.bashrc

**!!!警告!!! 避免使用r系列工具**

要注意使用类似 `rlogin`, `telnet`, `rsh` 和 `rcp` 等工具的危险。由于他们在网络上传输数据是未经过加密的所以他们本质上是不安全的。如果你需要远程执行和文件传输之类的工具，推荐使用SSH，从 http://www.openssh.org 下载，不同的客户端程序已经出现非UNIX系统上，请检查本地的软件镜象。 

#### 当UID不等于EUID时候调用

此种方式无起始文件读取。

### 交互shell

#### 什么是交互shell？

交互shell通常在用户终端读取并且输入：输入输出都连接到终端。Bash交互行为在 `bash` 命令不带选项参数时候调用，除了选项是从标准输入读取得字符串或者 交互式行为在 `bash` 命令以没有非选项参数调用时启动，除了选项是一个需要来读取的字符串或者当shell从读取允许设置位置参数的标准输入调用 (参见 [第 3 章 _Bash环境_](../Bash-Environment/README.md) ). 

#### 这个shell是交互的吗？

以察看特殊参数 `-` 的内容来测试，当shell是交互时包含了一个'i'：

```bash
echo $-
###himBH
```

在非交互shell中，提示，PS1，是没有设置的

#### 交互shell的行为

在交互模式的区别：

* Bash读取起始文件。
* 默认打开作业控制。
* 提示已经设置好， PS2 为多行命令开启，通常设置到 “>”。 同样也会提示你当shell认为你输入了一个不完整的命令，比如当你忘记引号，命令结构不能被省略，等。
* 默认使用 `readline` 从命令行读取命令。
* Bash解释shell选项 ignoreeof 而不是在接受到EOF之后立即退出。
* 默认情况下历史命令和历史扩展式开启的。历史在shell退出时都保存在 `HISTFILE` 指向的文件中。默认情况下，`HISTFILE` 指向 `~/.bash_history`。
* 别名扩展开启。
* 没有启动陷阱，`SIGTERM` 信号被忽略。
* 没有启动陷阱，`SIGINT` 被捕捉并被处理。因此，输入 Ctrl+C，比如，不会退出你的交互shell。
* 在退出时发送 `SIGHUP` 信号给所有的作业是以 `huponexit` 选项来设置的。
* 命令在读取完毕后才执行。
* Bash周期性地检查邮件。
* Bash可以配置成遇到未引用变量就退出。在交互模式这个行为是关闭的。
* 当shell内建命令遇到重定向错误，不会造成shell退出。
* 特殊内建命令在POSIX模式下使用返回错误不会导致shell退出。内建命令列在[第 1.3 节 “执行命令”](Executing-Commands.md)。
* `exec` 失败不会退出shell。
* 解释语法错误不会导致shell退出。
* 默认内建命令 cd 的参数简单拼写检查是开启的。
* 当定义 `TMOUT` 变量中的时间过去之后自动退出，是开启的。

更多信息：

* [第 3.2 节 “变量”](../Bash-Environment/Variables.md)
* [第 3.6 节 “更多 Bash 选项”](../Bash-Environment/More-Bash-Options.md)
* 参见 [第 12 章 _捕捉信号_](../Catching-Signals/README.md) 更多关于信号。
* [第 3.4 节 “Shell扩展”](../Bash-Environment/Shell-Extensions.md) 讨论在输入命令时执行的多种扩展。

### 条件

`[[` 复合命令以及 `test` 和 `[` 内建命令使用条件表达式。

表达式可以是一元或者二元的。一元表达式经常用来检验文件的状态。你只需要一个对象，比如一个文件，就能执行操作。

也有字符串操作符和数字比较操作符；这些都是二元操作符，需要两个对象来执行操作。如果对其中某个primaries FILE 参数是这样的形式 `/dev/fd/N`, 就检查文件描述符N。 如果对某个primaries FILE 参数是 `/dev/stdin`, `/dev/stdout` or `/dev/stderr`其中之一，那么就分别检查文件描述符0，1，2。

条件在 [第 7 章 _条件语句_](../Conditional-Statements/README.md)详细讨论。

关于文件描述符得更多信息，请参见 [第 8.2.3 节 “重定向和文件描述符”](../Writing-Interactive-Script/README.md)。 

### Shell运算

shell允许使用运算表达式作为一个shell扩展或者使用内建命令 `let` 求值。

赋值用定宽整数完成且不进行溢出检查，尽管除以0被捕获并标记为一个错误。操作符和它们的的优先级和结合性和在C语言中是一样的，参见 [第 3 章 _Bash环境_](../Bash-Environment/README.md). 

### 别名

别名允许一个字符串代替一个词当它用作一个简单命令的第一个词。shell保留一张可以用 `alias` 和 `unalias` 命令来设置和去除的别名列表。

Bash总是在执行某行的任何命令之前读取至少一个完整的输入行。别名在命令被读取的时候扩展开，而不是执行的时候。因此，一个别名定义作为另外一个命令出现在相同行在直到下一行被读取之前不会产生作用。该行跟在别名定义后的命令不会被新的别名影响。

当一个函数定义被读取时别名定义就扩展开来，而不是在函数在执行的时候，因为一个函数定义本身就是一个复合命令。因而，在函数执行之后，在函数内的别名定义就不存在了。

我们会在 [第 3.5 节 “别名”](../Bash-Environment/Alias.md) 具体讨论别名。 

### 数组

Bash提供一维数组变量。任何变量都可以用在数组中； `declare` 内建命令将明确地声明一个数组。一个数组的大小没有最大的限制，也没有成员索引或者连续赋值的要求。数组是基于0的。参见 [第 10 章 _变量进阶_](../Advanced-Variables/README.md)。

### 目录栈

目录栈是一个最近访问目录的列表。 `pushd` 内建命令把目录加入到栈中当切换到当前的目录的时候，`popd` 内建命令从栈中移除指定的目录且把当前目录转换到移除的目录。

内容可以用 `dirs` 命令来显示或者通过检查 `DIRSTACK` 变量的内容。

关于这种机制的工作情况可以查询Bash info页面来得到更多信息。

### 提示

Bash 使得使用提示变得更加有趣。参见Bash info页面的 _Controlling the Prompt_ 一节。 

### 受限制shell

当以 `rbash` 调用或者使用 `--restricted` 或 `-r` 选项，以下情况就发生：

* `cd` 内建命令被禁止。
* 不能设置和反设置 `SHELL`, `PATH`, `ENV` or `BASH_ENV`。
* 命令不能再包含斜杠。
* 包含斜杠的文件名不允许和 `.` (`source`) 内建命令一起使用。
* `hash` 内建命令不接受斜杠带 `-p` 选项。
* 在启动的时候输入函数禁止。
* `SHELLOPTS` 在启动时候被忽略。
* 输出重定向 `>`, `>|`, `><`, `>&`, `&>` 和 `>>` 禁止。
* `exec` 内建命令禁止。
* `-f` 和 `-d` 选项禁止在和内建命令 `enable` 使用。
* 默认的 `PATH` 不能用 `command` 内建命令来指定。
* 不能关闭限制模式。

当一个是shell脚本的命令执行的时候，`rbash` 关闭shell产生的任何限制来执行脚本。

更多信息：

* [第 3.2 节 “变量”](../Bash-Environment/Variables.md)
* [第 3.6 节 “更多 Bash 选项”](../Bash-Environment/More-Bash-Options.md)
* Info Bash → Basic Shell 特性 → 重定向
[第 8.2.3 节 “重定向和文件描述符”](../Writing-Interactive-Script/README.md)

