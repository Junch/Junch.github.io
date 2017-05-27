---
layout: post
title: "使用putty和switchysharp翻墙"
description: ""
tags: ["putty", "proxy"]
categories: ["技巧"]
---

## 一、获得SSH账号
这个是首要条件，否则后面的操作都没有意义。但如何获得SSH账号不在本文的表诉范围。

## 二、配置PuTTy

第一步：打开PuTTy，点击“Session”（打开默认就是此界面），出现如图所示的界面，按图中所示进行操作。
![](/images/putty_configure1.jpg)

**注：如果下次想继续使用，只要打开PuTTy，然后选择相应的对话名称，点击“Load”按钮即可。**

第二步：配置PuTTy。接上一步后，点击“Connection”→“SSH”→“Tunnels”，接着按下图所示进行操作即可：
![](/images/putty_configure2.jpg)

**注：在此我们使用的端口号是6600，一般我们在此所使用的端口号只要是大于1024的都可以。**

第三步：配置PuTTy。如下图所示，就是我们进行第二步操作后所示的界面：
![](/images/putty_configure3.jpg)

第四步：接下来就会出现如下图所示的PuTTy的登录界面：
![](/images/putty_configure4.jpg)

第五步：输入你空间的账户名和密码，即可以出现如下图所示的界面，由下图来看，我们已经成功登录了。
![](/images/putty_configure5.jpg)

## 三、配置switchysharp

switchysharp是chrome的一个插件，安装后作如下配置：

第一步：配置情景模式(Proxy Profiles)
![](/images/switchysharp1.png)

**注：端口号必须和PuTTy的设置保持一致**

第二步：配置切换规则(Switch Rules)

启用切换规则，使用在线规则列表http://autoproxy-gfwlist.googlecode.com/svn/trunk/gfwlist.txt
![](/images/switchysharp2.png)

## 四、使用密钥登录OpenSSH
请参考文章[putty使用密钥登陆OpenSSH](http://www.linuxfly.org/post/175/)

## 五、在Mac下使用SSH动态转发(使用密码)
在Mac下可以直接使用ssh命令

```bash
ssh user@ip -NfD port
```

- **-D** Specifies a local ''dynamic'' application-level port forwarding.
- **-f** Requests ssh to go to background just before command execution.
- **-N** Do not execute a remote command. This is useful for just forwarding ports (protocol version 2 only).

更详细的解释参考[man ssh](http://linux.die.net/man/1/ssh)。可以使用下面的命令查看ssh进程是否在运行
```bash
ps | grep ssh
```

## 六、在Mac下使用SSH动态转发(使用密钥)
在Windows环境下常使用putty登陆到远程Linux主机，其间使用了ppk文件。Mac下没有putty，但可以直接使用ssh命令，这个命令需要在terminal下来执行。在使用ssh前，需要把ppk文件的格式转换一下，方法是：仍然在Windows，打开puttygen.exe，读入ppk文件，然后点击Conversions菜单，选择Openssh，假定文件存为myppk.ssh。此时，把此文件传输到Mac后就可以在Mac下执行ssh来远程登陆了。我使用的命令格式为：

```bash
ssh user@ip -NfD port -i myppk.ssh
```

以上，user和ip需根据你的实际情况进行替换。myppk.ssh文件名也可以自己指定。另外，需要设置myppk.ssh的访问许可。

```bash
chmod 700 myppk.ssh
```

### 备注
PuTTy的内容来自[利用PuTTY的SSH Tunnels实现安全的代理](http://www.huluboke.com/putty-ssh-tunnels/)

另外部分内容来自[在Mac OS X下使用SSH登陆到远程服务器](http://www.linuxidc.com/Linux/2012-01/51021.htm)

手动安装switchysharp可参考[页面](http://switchysharp.com/install.html)
