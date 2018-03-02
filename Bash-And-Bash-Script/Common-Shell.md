# 常用的shell程序

## shell的普遍作用

UNIX的shell程序解释用户的命令，不管是用户直接输入的或者从一个称作Shell脚本或者Shell程序文件读入。Shell脚本是解释型的，而不是编译型的。Shell从脚本行的每行读取命令并在系统中搜索这些命令\(参见 [第 1.2 节 “Bourne Again SHell的优势”](Advantages-Of-Bash.md)\)，当编译器把一个程序转化为可供机器读取的形式时，那么它就可以被以一个可执行文件用在shell脚本当中。

除了向内核传送命令之外，shell的主要任务是提供一个可单独配置的使用shell资源配置文件的用户环境。

## Shell类型

就像一个人会知道不同的语言和方言一样，你的UNIX系统通常提供多种shell类型：

* `sh` 或者称作 Bourne Shell: 最初的shell并且仍然在UNIX系统和UNIX相关系统中使用。它是基本的shell，是一个特性不多的小程序。虽然不是一个标准的shell，但是为了UNIX程序的兼容性在每个Linux系统上仍然存在。
* `bash` 或者称作 Bourne Again shell: 标准的GNU shell，直观而又灵活。或许是初学者的最明智选择同时对高级和专业用户来说也是一个强有力的工具。在Linux上，`bash` 是普通用户的标准shell。这个shell因此称为Bourne shell的_超集_，一套附件和插件。意味着`bash`和`sh`是兼容的：在`sh`中可以工作的命令，在`bash`中也能工作，反之则不然。本书所有的例子和练习均使用`bash`。
* `csh` 或者称作 C shell: 语法了类似于C语言，某些时候程序员会使用。
* `tcsh` 或者称作 Turbo C shell: 普通C shell的超集，加强了的用户友好度和速度。
* `ksh` 或者称作 Korn shell: 某些时候被有UNIX背景的人所赏识。Bourne shell的一个超集，有着对初学者来说就是一场恶梦的标准配置。

文件 /etc/shells 给出了Linux系统上所有的已知shell的概览:

```console
$ cat /etc/shells
/bin/bash
/bin/sh
/bin/tcsh
/bin/csh
```

你默认的shell设置在 `/etc/passwd` 文件中，象下面这行对用户 _mia_ 的设置:

```
mia:L2NOfqdlPrHwE:504:504:Mia Maya:/home/mia:/bin/bash
```

要从一个shell转换到另外一个，只要在活动的终端里输入新shell的的名字。系统在PATH设置的目录里面寻找是否包含着个名字，既然一个shell就是一个可执行的文件（程序），当前的shell激活它使它运行起来，新的提示符出现，因为每个shell都有自己典型的外观：

```console
$ tcsh
[mia@post21 ~]$
```



