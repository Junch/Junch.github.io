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
```
windbg attach上去然后查找函数
x module!function
如果能查找到函数，可以直接打断点
bp module!function
如果module还没load, 没有查找到函数，可以执行
bu module!function
```

2、应用程序还没启动。

```
下面的脚本是在加载模块moduleA的时候中断。
windbg -Q -c "sxe ld moduleA;g" "path of the exe"
-Q suppresses the "Save Workspace?" dialog box.
-c "command" Specifies the inital debugger command to run at start-up.

在程序运行前就设置好断点
windbg -Q -c "bu module!function;g" "path of the exe" 
```

3、在某一行打断点

```
下面的命令是在file.cpp的108行打断点，这个文件file.cpp是在模块moduelA中定义的。
bp `moduleA!file.cpp:108`
```

4、条件断点
```
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
```

5、数据断点（data breakpoint or access breakpoint）
```
ba [r|w|e][size] Addr ; Break on Access: [r=read/write, w=write, e=execute], size=[1|2|4|8 bytes]
比如下面的例子
ba r4 myVar

```

6、给managed代码打断点
```
暂且留白，这部分内容待补充
```


### 调试如下程序

{% highlight cpp linenos %}
// Program name: hello.exe
// File name: hello.cpp

#include <stdlib.h>
#include <stdio.h>

int gNumber = 0;

__declspec(noinline)
void printHello(int i)
{
    printf(">> printHello\n");
    printf("%d, Hello World!\n",i);
    gNumber = i;
    printf("<< printHello\n");
}

__declspec(noinline)
int getNumber(int i) {
    return 2*i+1;
}

int
main( int /*argc*/, char** /*argv*/ )
{
    for (int i=0; i<10; ++i){
        printHello(getNumber(i));
    }

    return 0;
}
{% endhighlight %}

首先采用如下命令进行编译链接。
```bat
cl /EHa /Zi /Ox /favor:INTEL64 hello.cpp /link /debug
```

给程序打下如下断点

1. 在hello.cpp的第27行打下断点
2. 对于printHello函数，当参数i为11时让程序停下来
3. 对于getNumber函数，当函数的返回值为7的时候让程序停下来
4. 对于printHello函数，当参数i大于11后，不再执行该函数
5. 让getNumber返回一个固定的值比如10
6. 当gNumber被设置成13的时候，打断点

答案如下

```bat
1. bp `hello!hello.cpp:27`
2. bp hello!printHello ".if (i=0n11) {} .else {gc}" 或
   bp hello!printhello ".if (i==0n11) {} .else {gc}" 或
   bp hello!printhello ".if (@ecx=0n11) {} .else {gc}" 或
   bp hello!printhello ".if (@ecx==0n11) {} .else {gc}"
3. bp hello!getNumber "gu; .if (@eax=0n7) {} .else {gc}"
4. bp hello!printHello ".if (@ecx > 0n11) {r rip=poi(@rsp); r rsp=@rsp+0x8}; gc"
5. bp hello!getNumber "gu; r rax=0n10; g"
6. hello!gNumber ".if (dwo(hello!gNumber) == 0n13) {} .else {gc}"
```






