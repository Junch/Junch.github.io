---
layout: post
title: "Windbg调试（1）- pdb"
summary: "调试的基础是获得符号表"
description: ""
tags: ["windbg"]
categories: ["Debug"]
---

在windows下调试，首要的是获得Symbols。在开发环境下，开发者自己编译程序就可以得到pdb, 所生成的Binary会自动设置符号表的地址指向这个文件，调试器很容易就可以获得。但如果针对安装版本，对其调试就必须设置Symbol Path，告诉调试器哪里去寻找这些pdb。

做几个简单的实验查询pdb文件路径。实验前需要先从以下网址下载Debugging Tools for Windows，里面包含windbg, symchk等工具。

- [https://msdn.microsoft.com/en-us/windows/hardware/hh852365](https://msdn.microsoft.com/en-us/windows/hardware/hh852365)
- [http://msdn.microsoft.com/en-us/windows/hardware/dn913721.aspx](http://msdn.microsoft.com/en-us/windows/hardware/dn913721.aspx)

### 用SymChk查看nodepad的pdb文件路径

执行命令
C:\symchk  c:\Windows\notepad.exe /v
得到下面的结果：

{% highlight text %}
C:\>symchk  c:\Windows\notepad.exe /v
[SYMCHK] Searching for symbols to c:\Windows\notepad.exe in path SRV*C:\WINDOWS\SYMBOLS*http://msdl.microsoft.com/download/symbols
DBGHELP: Symbol Search Path: SRV*C:\WINDOWS\SYMBOLS*http://msdl.microsoft.com/download/symbols
[SYMCHK] Using search path "SRV*C:\WINDOWS\SYMBOLS*http://msdl.microsoft.com/download/symbols"
DBGHELP: No header for c:\Windows\notepad.exe.  Searching for image on disk
DBGHELP: c:\Windows\notepad.exe - OK
DBGHELP: notepad - public symbols
         C:\WINDOWS\SYMBOLS\notepad.pdb\57060987A4344E1A9B9B77F57D14388A2\notepad.pdb
[SYMCHK] MODULE64 Info ----------------------
[SYMCHK] Struct size: 1680 bytes
[SYMCHK] Base: 0x0000000100000000
[SYMCHK] Image size: 217088 bytes
[SYMCHK] Date: 0x559ea8be
[SYMCHK] Checksum: 0x00036da2
[SYMCHK] NumSyms: 0
[SYMCHK] SymType: Sympdb
[SYMCHK] ModName: notepad
[SYMCHK] ImageName: c:\Windows\notepad.exe
[SYMCHK] LoadedImage: c:\Windows\notepad.exe
[SYMCHK] pdb: "C:\WINDOWS\SYMBOLS\notepad.pdb\57060987A4344E1A9B9B77F57D14388A2\notepad.pdb"
[SYMCHK] CV: RSDS
[SYMCHK] CV DWORD: 0x53445352
[SYMCHK] CV Data:  notepad.pdb
[SYMCHK] pdb Sig:  0
[SYMCHK] pdb7 Sig: {57060987-A434-4E1A-9B9B-77F57D14388A}
[SYMCHK] Age: 2
[SYMCHK] pdb Matched:  TRUE
[SYMCHK] DBG Matched:  TRUE
[SYMCHK] Line nubmers: FALSE
[SYMCHK] Global syms:  FALSE
[SYMCHK] Type Info:    FALSE
[SYMCHK] ------------------------------------
SymbolCheckVersion  0x00000002
Result              0x00030001
DbgFilename
DbgTimeDateStamp    0x559ea8be
DbgSizeOfImage      0x00035000
DbgChecksum         0x00036da2
pdbFilename         C:\WINDOWS\SYMBOLS\notepad.pdb\57060987A4344E1A9B9B77F57D14388A2\notepad.pdb
pdbSignature        {57060987-A434-4E1A-9B9B-77F57D14388A}
pdbDbiAge           0x00000002
[SYMCHK] [ 0x00000000 - 0x00030001 ] Checked "c:\Windows\notepad.exe"

SYMCHK: FAILED files = 0
SYMCHK: PASSED + IGNORED files = 1
{% endhighlight %}

在C:\WINDOWS\SYMBOLS目录下你会发现下载的notepad.pdb文件。

### 用SymChk查看node的pdb文件路径
最近对node.js有些着迷，就以node为例来查找其pdb文件路径

{% highlight text %}
C:\>where node
C:\Program Files\nodejs\node.exe

C:\>symchk  "c:\Program Files\nodejs\node.exe" /v
[SYMCHK] Searching for symbols to c:\Program Files\nodejs\node.exe in path SRV*C:\WINDOWS\SYMBOLS*http://msdl.microsoft.com/download/symbols
DBGHELP: Symbol Search Path: SRV*C:\WINDOWS\SYMBOLS*http://msdl.microsoft.com/download/symbols
[SYMCHK] Using search path "SRV*C:\WINDOWS\SYMBOLS*http://msdl.microsoft.com/download/symbols"
DBGHELP: No header for c:\Program Files\nodejs\node.exe.  Searching for image on disk
DBGHELP: c:\Program Files\nodejs\node.exe - OK
SYMSRV:  C:\WINDOWS\SYMBOLS\node.pdb\5DAD44455A83417DBDB6BE0655EE650D1\node.pdb not found
SYMSRV:  http://msdl.microsoft.com/download/symbols/node.pdb/5DAD44455A83417DBDB6BE0655EE650D1/node.pdb not found
DBGHELP: node - no symbols loaded
[SYMCHK] MODULE64 Info ----------------------
[SYMCHK] Struct size: 1680 bytes
[SYMCHK] Base: 0x0000000140000000
[SYMCHK] Image size: 11161600 bytes
[SYMCHK] Date: 0x551b2623
[SYMCHK] Checksum: 0x00aa62c8
[SYMCHK] NumSyms: 0
[SYMCHK] SymType: SymNone
[SYMCHK] ModName: node
[SYMCHK] ImageName: c:\Program Files\nodejs\node.exe
[SYMCHK] LoadedImage: c:\Program Files\nodejs\node.exe
[SYMCHK] pdb: ""
[SYMCHK] CV: RSDS
[SYMCHK] CV DWORD: 0x53445352
[SYMCHK] CV Data:  d:\jenkins\workspace\nodejs-msi-julien\d8c2e2bb\Release\node.pdb
[SYMCHK] pdb Sig:  0
[SYMCHK] pdb7 Sig: {00000000-0000-0000-0000-000000000000}
[SYMCHK] Age: 0
[SYMCHK] pdb Matched:  TRUE
[SYMCHK] DBG Matched:  TRUE
[SYMCHK] Line nubmers: FALSE
[SYMCHK] Global syms:  FALSE
[SYMCHK] Type Info:    FALSE
[SYMCHK] ------------------------------------
SymbolCheckVersion  0x00000002
Result              0x00010001
DbgFilename         node.dbg
DbgTimeDateStamp    0x00000000
DbgSizeOfImage      0x00000000
DbgChecksum         0x00000000
pdbFilename         d:\jenkins\workspace\nodejs-msi-julien\d8c2e2bb\Release\node.pdb
pdbSignature        {5DAD4445-5A83-417D-BDB6-BE0655EE650D}
pdbDbiAge           0x00000001
[SYMCHK] [ 0x00000000 - 0x00010001 ] Checked "c:\Program Files\nodejs\node.exe"
SYMCHK: node.exe             FAILED  - node.pdb mismatched or not found

SYMCHK: FAILED files = 1
SYMCHK: PASSED + IGNORED files = 0
{% endhighlight %}

很明显在这种情况下是无法查找到pdb文件的，但通过这个命令可以获得一些信息，比如这个node.exe所对应的pdb
文件最初的位置，pdbFilename在d:\jenkins\workspace\nodejs-msi-julien\d8c2e2bb\Release\node.pdb。

如果你通过某种渠道得到了这个node.pdb,你可以按照这个路径放置node.pdb, 然后就可以调试了：）

### 用Windbg查看pdb文件路径 - 方法(1)
这里以百度云管家为例，启动Windbg并Attach上百度云管家，执行命令`!lmi`

{% highlight text %}
0:000> !lmi baiduyunguanjia
Loaded Module Info: [baiduyunguanjia] 
         Module: BaiduYunGuanjia
   Base Address: 0000000000360000
     Image Name: C:\Users\chenju\AppData\Roaming\Baidu\BaiduYunGuanjia\BaiduYunGuanjia.exe
   Machine Type: 332 (I386)
     Time Stamp: 55f9de3e Thu Sep 17 05:25:18 2015
           Size: 59a000
       CheckSum: 58a857
Characteristics: 102  
Debug Data Dirs: Type  Size     VA  Pointer
             CODEVIEW    88, 4079d8,  406dd8 RSDS - GUID: {DB20D638-5930-4F7A-BA34-2D64C7A4491A}
               Age: 1, pdb: D:\cygwin\home\scmpf\compiler_src\jinjiqiang_1581659_win64\0\mc\wangpan\windows\yunbrowser\output\pdb\YunUi.pdb
                   ??    14, 407a60,  406e60 [Data not mapped]
     Image Type: FILE     - Image read successfully from debugger.
                 C:\Users\chenju\AppData\Roaming\Baidu\BaiduYunGuanjia\BaiduYunGuanjia.exe
    Symbol Type: NONE     - pdb not found from image path.
    Load Report: no symbols loaded
{% endhighlight %}

### 用Windbg查看pdb文件路径 - 方法(2)
结合使用`!sym`和`reload`

{% highlight text %}
0:043> !sym noisy
noisy mode - symbol prompts on
0:043> .reload /f BaiduYunGuanjia.exe

SYMSRV:  c:\symbols\YunUi.pdb\DB20D63859304F7ABA342D64C7A4491A1\YunUi.pdb not found
SYMSRV:  http://msdl.microsoft.com/download/symbols/YunUi.pdb/DB20D63859304F7ABA342D64C7A4491A1/YunUi.pdb not found
DBGHELP: C:\Users\chenju\AppData\Roaming\Baidu\BaiduYunGuanjia\YunUi.pdb - file not found
DBGHELP: D:\cygwin\home\scmpf\compiler_src\jinjiqiang_1581659_win64\0\mc\wangpan\windows\yunbrowser\output\pdb\YunUi.pdb - file not found
*** ERROR: Module load completed but symbols could not be loaded for C:\Users\chenju\AppData\Roaming\Baidu\BaiduYunGuanjia\BaiduYunGuanjia.exe
DBGHELP: BaiduYunGuanjia - no symbols loaded

************* Symbol Loading Error Summary **************
Module name            Error
BaiduYunGuanjia        The system cannot find the file specified : srv*c:\symbols*http://msdl.microsoft.com/download/symbols
				The SYMSRV client failed to find a file iån the UNC store, or there
				is an invalid UNC store (an invalid path or the pingme.txt file is
				not present in the root directory), or the file is present in the
				symbol server exclusion list.
{% endhighlight %}

### 用Windbg查看pdb文件路径 - 方法(3)
使用命令`!chksym`或者是`!itoldyouso`，发现两者的结果一样

{% highlight text %}
0:043> !chksym baiduyunguanjia

C:\Users\chenju\AppData\Roaming\Baidu\BaiduYunGuanjia\BaiduYunGuanjia.exe
    Timestamp: 55F9DE3E
  SizeOfImage: 59A000
          pdb: D:\cygwin\home\scmpf\compiler_src\jinjiqiang_1581659_win64\0\mc\wangpan\windows\yunbrowser\output\pdb\YunUi.pdb
      pdb sig: DB20D638-5930-4F7A-BA34-2D64C7A4491A
          age: 1
{% endhighlight %}


参考阅读以下链接：

1. [http://stackoverflow.com/questions/18756009/get-pdb-file-path-from-windbg](http://stackoverflow.com/questions/18756009/get-pdb-file-path-from-windbg)
2. [PDB Files: What Every Developer Must Know](http://www.wintellect.com/devcenter/jrobbins/pdb-files-what-every-developer-must-know)
3. [Setting the WinDbg Symbol Search Path (article + video)](https://www.osr.com/blog/2014/10/01/setting-the-windbg-symbol-search-path/)




