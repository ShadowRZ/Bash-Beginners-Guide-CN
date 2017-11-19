# 总结
Bash是GNU shell，兼容`sh`以及其他shell里的许多有用的特性。当shell启动的时候，它读取它自己的配置文件。最重要的几个如下所示：

* `/etc/profile`

* `~/.bash_profile`

* `~/.bashrc`

Bash在交互模式下，遵循POSIX标准的时候和限制模式下的行为会有所不同。

shell命令可以分成为3个种类：shell的功能命令，shell内建命令和你系统里某个目录里的命令。Bash支持额外的sh不包含的内建命令。Shell脚本把那些命令组成shell语法命令。脚本读取一行执行一行且应该有逻辑结构。

Shell脚本把那些命令组成shell语法命令。脚本读取一行执行一行且应该有逻辑结构。