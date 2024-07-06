---
title: 使用 gdb 工具调试 C 程序
date: 2016-02-18 17:25:15
tags:
- gdb
- c
- debug
---
gdb 是 C 编程中调试程序的一个强大工具. 程序开发中经常需要一些调试方法来对程序进行分析 debug.

通常我们检查程序的错误只是简单地根据执行时的出错现象假设错误原因，然后在代码中适当的插入 printf, 执行程序并分析打印结果，如果结果和预期的一样，就基本上证明了自己假设的错误原因，就可以动手修正 bug 了，如果结果与预期的不一样，就要进一步的假设和分析. 这样的方式对于小型程序还能应付之，但当面对代码量巨大的程序时，我们就急切需要一种工具来更详细具体地观察程序运行过程中各个变量以及函数的调用信息，查看程序中所有的内部状态，比如变量的值，传给函数的参数，当前执行的代码行等. `gdb` 就是这样一个工具，掌握它的用法后，我们调试程序的手段就更加丰富了. 即使如此，也应该始终记得调试程序的的基本思想仍然是"分析现象－假设错误原因－产生新的现象去验证假设"这样一个循环，根据现象如何假设错误原因，以及如何设计新的现象去验证假设，这都需要非常严密的分析和思考.

> 此博客是学习过程中的一些简短记录，并不能梳理成文。如果你想要了解更多关于 gdb 的信息，还请 google之.

#### 预热
在编译时需要加上`-g`选项，生成的可执行文件才能用`gdb`进行源码级调试.
```sh
lechance@debian:/tmp$ cc -g bp.c -o bp
lechance@debian:/tmp$ gdb bp
GNU gdb (Debian 7.7.1+dfsg-5) 7.7.1
Copyright (C) 2014 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "i586-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from bp...done.
(gdb) 
```
`-g`选项的作用是在可执行文件中加入源代码的信息，比如可执行文件中第几条指令对应源代码的第几行，但并不是把整个文件嵌入到可执行文件中，所以才调试时必须保证`gdb`能找到源文件.`gdb`提供一个类似shell的命令行环境，上面的gdb就是提示符，在这个提示符下可以输入help查看命令的类别.
```sh
(gdb) help
List of classes of commands:

aliases -- Aliases of other commands
breakpoints -- Making program stop at certain points
data -- Examining data
files -- Specifying and examining files
internals -- Maintenance commands
obscure -- Obscure features
running -- Running the program
stack -- Examining the stack
status -- Status inquiries
support -- Support facilities
tracepoints -- Tracing of program execution without stopping the program
user-defined -- User-defined commands

Type "help" followed by a class name for a list of commands in that class.
Type "help all" for the list of all commands.
Type "help" followed by command name for full documentation.
Type "apropos word" to search for commands related to "word".
Command name abbreviations are allowed if unambiguous.

```
#### 常用命令
##### list [row_number]
从指定的row_number行开始列出源代码，也可以忽略指定显示的行号，list命令也可以简化为l使用.
##### Enter key
gdb提供了一个方便的功能，在提示符下直接敲回车键重复上一条命令.
##### sart
我们使用start命令开始执行程序，通过在提示符下查看start的help，我知道了还可以在start时指定程序启动的参数.
```sh
(gdb) help start
Run the debugged program until the beginning of the main procedure.
You may specify arguments to give to your program, just as with the
"run" command.
```
##### next (abbreviation: n)
我们可以使用next命令控制程序的语句一条一条地执行. gdb停在main函数中变量定义之后的第一条语句处等待我们发出命令.
```sh
(gdb) start
Temporary breakpoint 1 at 0x804845c: file bp.c, line 4.
Starting program: /tmp/bp 

Temporary breakpoint 1, main () at bp.c:4
4	    int sum = 0, i = 0;
(gdb) n
8		scanf("%s", input);
(gdb) 
```
##### step (abbreviation: s)
执行下一行语句，如果有函数调用则进入到函数中
```sh
(gdb) s
add_range (low=1, high=10) at main.c:6
6		for (i = low; i <= high; i++)
```
##### backtrace (abbreviation: bt)
使用`backtrace`命令可以查看函数调用的栈帧.
```sh
(gdb) bt
#0  add_range (low=1, high=10) at main.c:6
#1  0x080483c1 in main () at main.c:14
```
#### info (abbrevition: i)
用info命令（简写为i）查看add_range函数局部变量的值：
```sh
(gdb) i locals
i = 0
sum = 0
```
如果想查看main函数当前局部变量的值也可以做到，先用frame命令（简写为f）选择1号栈帧然后再查看局部变量：
```sh
(gdb) f 1
#1  0x080483c1 in main () at main.c:14
14		result[0] = add_range(1, 10);
(gdb) i locals 
result = {0, 0, 0, 0, 0, 0, 134513196, 225011984, -1208685768, -1081160480, 
...
  -1208623680}
```
#### print (abbrevition: p)
打印表达式的值，通过表达式可以修改变量的值或者调用函数
```sh
(gdb) p sum
$1 = 3
```
#### [un]display [variables]
用display命令使得每次停下来的时候都显示当前sum的值
```sh
(gdb) display sum
1: sum = -1208103488
```
#### break (b) [row_number]
在某一行设置断点

