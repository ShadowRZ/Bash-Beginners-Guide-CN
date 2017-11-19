# 练习

这里有些联系可以让你在进入下一章节之前进行下热身：

1. `bash` 程序位于你系统的什么位置？
2. 使用 `--version` 选项来找到你正在运行的版本。
3. 当使用图形用户界面进入系统然后打开终端窗口时哪个shell配置文件会被读取？
4. 以下shell是交互shell吗？它们是登陆shell吗？
 * 一个shell通过你的图形桌面打开，从菜单选择类似 “Terminal” 。
 * 一个你执行 `ssh localhost` 命令之后得到的shell。
 * 一个在你以文本模式登陆进控制台得到的shell。
 * 一个通过命令 `xterm &` 得到的shell。
 * 一个由 `mysystem.sh` 脚本打开的shell。
 * 一个登陆远程主机上的shell，由于你使用了SSH或者SSH keys而不需要输入用户或者密码。
5. 你能解释为什么 `bash` 当你输入 Ctrl+C 的时候没有退出？
6. 显示目录堆栈内容。

7. 如果还不是这样的话，设置提示符来显示在文件系统结构中你的位置，比如加入这行到文件 `~/.bashrc`：
 ```bash
 export PS1="\u@\h \w> "
 ```
8. 显示当前shell任务的hash命令。
9. 当前有多少进程正在你的系统中运行？使用 `ps` 和 `wc`， `ps` 的第一行输出不是进程。
10. 怎么显示系统的主机名称？只要名字，其他都不需要显示。