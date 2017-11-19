# 脚本基础

## 哪个Shell来执行脚本？

当在子shell运行脚本时，你应该定义哪个shell来运行脚本，你编写的脚本的shell类型可能不是你系统默认的，所以用错误的shell来运行你输入的命令可能最终出错。

第一行决定了启动的shell，第一行的开始2个字符应该是`#!`，然后紧跟解释后面命令的shell的路径。空白行也被认为是一行，所以不要让你的脚本以空白行开始。

出于本教程的考虑，所有的脚本都这样开头：

```bash
#!/bin/bash
```

和先前提到过的一样，这样表明Bash可以在 `/bin` 里面找到。 

## 加入注释

你应该知道事实上你不会阅读你自己脚本的唯一的一个人。很多用户和系统管理员运行别人编写的脚本。如果他们想知道你是如何做到的，注释能很好的提醒读者。

注释也同样让你自己更方便。你一定阅读了很多帮助页面通过脚本中的一些命令来得到特定的结果。如果不对脚本加上注释，几个星期或者几个月后你需要更改你的脚本，你会忘记脚本做了些什么事，你怎么做的和为什么要这么做。

把 `script1.sh` 例子复制到 [`commented-script1.sh`](https://github.com/ShadowRZ/Bash-Beginners-Guide-CN/blob/master/Scripts/commented-script1.sh)，编辑后反映了脚本的行为。在`#`后面的行被忽略而且只能在打开脚本时才能看到： 

```bash
#!/bin/bash
# This script clears the terminal, displays a greeting and gives information
# about currently connected users.  The two example variables are set and displayed.

clear				# clear terminal window

echo "The script starts now."

echo "Hi, $USER!"		# dollar sign is used to get content of variable
echo

echo "I will now fetch you a list of connected users:"
echo							
w				# show who is logged on and
echo				# what they are doing

echo "I'm setting two variables now."
COLOUR="black"					# set a local shell variable
VALUE="9"					# set a local shell variable
echo "This is a string: $COLOUR"		# display content of variable 
echo "And this is a number: $VALUE"		# display content of variable
echo

echo "I'm giving you back your prompt now."
echo
```

在一个良好的脚本中，第一行经常注明要完成的任务。然后为了明确每一大块命令将被加上注释。作为一个例子，Linux的初始脚本，在你系统里的 `init.d` 目录下，为了能让每个使用Linux的人读取和更改它，通常都作了良好的注释。 
