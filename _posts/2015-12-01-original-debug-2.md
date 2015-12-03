---
layout: post
title: "Windbg调试（2）- 断点"
summary: "查询变量的值"
description: ""
tags: ["windbg"]
categories: ["Debug"]
---

获得PDB文件后，接着是要让程序停下来，也就是给程序打断点。打断点的方式多种多样，自己曾经采用的有下列方式：

1、应用程序已经启动的情况。
{% highlight text %}
windbg attach上去然后查找函数
x module!function
如果能查找到函数，可以直接打断点
bp module!function
如果module还没load, 没有查找到函数，可以执行
bu module!function
{% endhighlight %}

2、应用程序还没启动。

{% highlight text %}
下面的脚本是在加载模块moduleA的时候中断。
windbg -Q -c "sxe ld moduleA;g" "path of the exe"
-Q suppresses the "Save Workspace?" dialog box.
-c "command" Specifies the inital debugger command to run at start-up.

在程序运行前就设置好断点
windbg -Q -c "bu module!function;g" "path of the exe" 
{% endhighlight %}

3、在某一行打断点

{% highlight text %}
下面的命令是在file.cpp的108行打断点，这个文件file.cpp是在模块moduelA中定义的。
bp `moduleA!file.cpp:108`
{% endhighlight %}

4、条件断点
{% highlight text %}
函数的第一个参数是2的时候中断，注意不能使用单引号
bp module!function ".if(@rcx = 2) {} .else {gc}"

函数返回值为7的时候中断
bp module!function "gu; .if(@rax = 7) {} .else {gc}"

在断点处打印函数的callstack
bp module!function ".if(@rcx = 2) {kn} .else {gc}"

在断点处打印某个register
bp module!function ".if(@rcx = 2) {?? @rdx} .else {gc}"

在断点处甚至可以直接修改register
bp `module!function:linenumber` "r @rip=address; gc"
{% endhighlight %}

5、给managed代码打断点
{% highlight text %}
暂且留白，这部分内容待补充
{% endhighlight %}
