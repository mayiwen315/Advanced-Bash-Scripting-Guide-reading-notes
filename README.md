 Advanced-Bash-Scripting-Guide-reading-notes
 ==============
第一部分热身<br>
===========
1.为什么使用<br>
------------
>>1.1 Shell编程就是把各个命令和工具连接起来。以下情况下不适合使用shell编程：资源密集型任务，处理大型数学任务，跨平台，复杂、重要、安全要求很高的应用，需要图形界面，连接I/O和socket接口，需要多维数组等。<br>

2.建议
-----
2.1 收集一些比较有代表性的 "模版"代码。注意代码的健壮性，使用变量取代硬代码，将重复使用的代码放入函数。<br>
2.2 使用sh，bash可以调用脚本，还可以改变权限(555、+rx、u+rx)然后通过./执行<br>

第二部分基础
==========
3.特殊字符
---------------
3.1	;;终止case
. 等价于source命令. sourc一个文件(或点命令)将会在脚本中引入代码, 并将这些代码附加到脚本中(与C语言中的#include指令效果相同) "点"作为文件名的一部分. 如果点放在文件名的开头的话, 那么这个文件将会成为"隐藏"文件, 并且ls命令将不会正常的显示出这个文件。
: 在与>重定向操作符结合使用时, 将会把一个文件清空, 如果之前这个文件并不存在, 那么就创建这个文件。
()命令组（子shell，变量在其他部分不可用）或初始化数组。
{}创建匿名函数，但是与一般函数不同的是其中变量外面可以看到。<br>
.>|强制重定向，这将强制的覆盖一个现存文件

4.变量和参数的介绍
--------------
4.1	双引号弱引用，里面的有些变量可能会进行变量替换(所以在使用时注意空格不要乱加)，单引号就是强引用。

5.引用
--------

6.退出和退出状态码
--------
exit 被用来结束一个脚本, 就像在C语言中一样. 它也返回一个值, 并且这个值会传递给脚本的父进程, 父进程会使用这个值做下一步的处理.每个命令都会返回一个 退出状态码 (有时候也被称为 返回状态 ). 成功的命令返回0, 而不成功的命令返回非零值。$?保存了最后所执行的命令的退出状态码。

7.条件测试结构
--------
7.1	if grep -q Bash file不打印任何标准输出。如果有匹配的内容则立即返回状态值0。test命令在Bash中是内建命令, 用来测试文件类型, 或者用来比较字符串。同[]。[[ ]]结构比[]结构更加通用。
7.2	一些参数解释，不详细列举了

8.操作符与相关主题
-------
8.1	数字常量
10进制: 默认情况，8进制: 以'0'(零)开头，16进制: 以'0x'或者'0X'开头的数字，其他以进制#数的形式。

第三部分进阶
========
9.变量重游
--------
9.1	$0, $1, $2, 位置参数 $#命令行参数 [2] 或者位置参数的个数$*所有的位置参数都被看作为一个单词. $@与$*相同, 但是每个参数都是一个独立的引用字符串。
9.2	Expr用于操作字符串
Awk命令
9.3	
9.4	指定变量的类型: 使用declare或者typeset
9.5	a=letter_of_alphabet letter_of_alphabet=z eval a=\$$a间接引用，类似c中的指针
9.6	$RANDOM: 产生随机整数范围在0 - 32767之间不应该被用来产生密匙。
9.7	((...))与let命令很相似, ((...))结构允许算术扩展和赋值。C语言风格。

10.循环与分支
--------
10.1	for arg in [list] ; do
set
set命令用来修改内部脚本变量的值. 它的一个作用就是触发选项标志位来帮助决定脚本的行为. 另一个作用是以一个命令的结果(set `command`)来重新设置脚本的位置参数. 脚本将会从命令的输出中重新分析出位置参数.
加一个循环的例子
加一个case例子
.#! /bin/bash
case "$1" in
"") echo "Usage: ${0##*/} <filename>"; exit $E_PARAM;;  # 没有命令行参数,
                                           # 或者第一个参数为空.
.# 注意: ${0##*/} 是 ${var##pattern} 的一种替换形式. 得到的结果为$0.
-*) FILENAME=./$1;;   #  如果传递进来的文件名参数($1)以一个破折号开头, 
.#+ 那么用./$1来代替.
.#+ 这样后边的命令将不会把它作为一个选项来解释.
* ) FILENAME=$1;;     # 否则, $1.
Esac
  
11.内部命令与内建命令
--------
内建命令将比外部命令执行的更快, 一部分原因是因为外部命令通常都需要fork出一个单独的进程来执行 -- 另一部分原因是特定的内建命令需要直接访问shell的内核部分。
例如echo，echo命令可以作为输入, 通过管道传递到一系列命令中去。
Printf，最主要的应用就是格式化错误消息。
Read，从stdin中"读取"一个变量的值, 也就是, 和键盘进行交互, 来取得变量的值。没有变量分配给'read'命令, 所以输入将分配给默认变量, $REPLY。
let命令将执行变量的算术操作. 在许多情况下, 它被看作是复杂的expr命令的一个简化版本。
set命令用来修改内部脚本变量的值。它的一个作用就是触发选项标志位来帮助决定脚本的行为。另一个作用是以一个命令的结果(set `command`)来重新设置脚本的位置参数. 脚本将会从命令的输出中重新分析出位置参数。
exec这个shell内建命令将使用一个特定的命令来取代当前进程. 一般的当shell遇到一个命令, 它会forks off一个子进程来真正的运行命令。 使用exec内建命令, shell就不会fork了, 并且命令的执行将会替换掉当前shell。因此, 在脚本中使用时, 一旦exec所执行的命令执行完毕, 那么它就会强制退出脚本。
type [cmd]与外部命令which很相像, type cmd将会给出"cmd"的完整路径.。与which命令不同的是, type命令是Bash内建命令。-a是type命令的一个非常有用的选项, 它用来鉴别参数是关键字还是内建命令, 也可以用来定位同名的系统命令。
读后感：目前只读完了基础部分和部分进阶内容，编程代码最重要的还是在于实际的应用和练习。本身是打算一直看完的，但由于一直在看的中文翻译版的网站突然服务器宕机，真的是....再找一个把剩下的读完吧....接下来把重点在于练习，会把例子敲打试一试，加油.....
