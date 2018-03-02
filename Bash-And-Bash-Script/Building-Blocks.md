# 建立块

## Shell建立块

### Shell语法

如果输入没有被注释掉，那么shell将读取并且把它分割成字和操作符，使用引用规则来定义输入的每个字符的意义。然后词和操作符就被转化为命令和其它结构，并能返回一个可供检查和处理的退出状态。以上fork-and-exec机制只有在以下列方法shell扫描输入后起作用：

* shell从文件，字符串或者用户终端来读取它的输入。

* 输入用字和操作符分割开，遵守引号规则，参见 [第 3 章 _Bash环境_](../Bash-Environment/README.md)。 这些记号使用 _特殊字符_ 来分隔。别名扩展也执行。

* shell _解析_ \(搜索和替换\) the tokens 成简单复合的命令。

* Bash执行多种shell扩展，把扩展记号分开成文件名的列表和命令以及参数。

* 重定向在需要时进行实现，重定向操作符合它们的操作数被从参数列表中移开。

* 命令执行后。

* Optionally shell等待命令结束并收集它的退出状态。

### Shell命令

一个简单的shell命令例如 `touch file1 file2 file3` 由命令本身，参数以及空格构成。

更复杂的shell命令由简单的命令以多种方式组织在一起：例如管道把一个命令的输出变成另外一个命令的输入，循环或者条件结构，或者其他的组织方式。请看一些例子：

```console
$ ls | more

$ gunzip file.tar.gz | tar xvf -
```

### Shell函数

Shell函数是一种为一系列稍后执行的命令命名的方法。他们执行起来就像 “普通” 命令。当一个Shell函数的名字被当作一个简单命令使用后，和函数名字相关联的命令就会执行。

Shell函数在当前shell的上下文执行；没有新的进程会建立来解释它们

函数会在 [第 11 章 _函数_](../Functions/README.md) 进行详细说明。

### Shell参数

参数是一个存储值的实体。可以是一个名字，一个数字，或者一个特殊的值。因为shell，一个变量是一个存储了名字的参数。一个变量可以是一个值或者是另或者其他属性。变量使用shell内建命令 `declare` 来建立。

如果不进行赋值，变量就被赋予空字符串。变量只能使用内建命令 `unset` 来移除。

变量赋值在 [第 3.2 节 “变量”](../Bash-Environment/Variables.md) 讨论，变量高级使用在 [第 10 章 _变量进阶_](../Advanced-Variables/README.md)。

### Shell扩展

Shell扩展在每个命令行被分隔成记号（tokens）后执行。这些是执行的扩展：

* 括号扩展
* `~` 扩展
* 参数和变量扩展
* 命令替换
* 算术扩展
* 词分割Word splitting
* 文件名扩展

我们会在 [第 3.4 节 “Shell扩展”](../Bash-Environment/Shell-Extensions.md) 具体讨论扩展的类型。

### 重定向

在一个命令执行之前，它的输入和输出可能重定向使用一个由shell解释的特殊符号。重定向也可以用为当前shell执行环境来打开和关闭文件。

### 执行命令

当执行一个命令时，When executing a command, the words that the parser has marked as variable assignments \(preceding the command name\) and redirections are saved for later reference. Words that are not variable assignments or redirections are expanded; the first remaining word after expansion is taken to be the name of the command and the rest are arguments to that command. Then redirections are performed, then strings assigned to variables are expanded. If no command name results, variables will affect the current shell environment.

shell任务的一个重要部分是搜索命令。Bash是按照下面的步骤来完成的：

* 检查命令是否包含斜杠。如果没有，首先检查函数列表是否包含一个我们寻找的命令。
* 如果命令不是一个函数，那么在内建命令列表中检查。
* 如果命令既不是函数也不是内建命令，那么扫描列在 `PATH` 中的目录列表来进行查找。Bash使用一个 _hash table_ \(内存中的数据存放区域\) 来记忆可执行文件的完整路径，这样能防止对 `PATH` 的扩展搜索。
* 如果搜索没有成功，Bash打印一条错误消息并返回退出状态127。
* 如果搜索成功或者命令包含一个斜杠，shell在一个单独执行环境中执行这个命令。
* 如果因为文件是不可执行的或者不是一个目录造成执行失败，就假设是一个shell脚本。
* 如果命令是异步开始的，shell会等待命令完成并收集它的退出状态。

### Shell脚本

调用`bash`时，当一个包含shell命令的文件被用作第一个非选项参数时候（不带 `-c` 或者 `-s`，这两个参数将会创建一个非交互的shell）。这个shell首先搜寻脚本文件所在的当前目录，如果没有找到则对环境变量 PATH 进行查找。

