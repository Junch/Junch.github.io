---
layout: post
title: "使用putty和switchysharp翻墙"
description: ""
tags: ["putty", "proxy"]
categories: ["技巧"]
---

##一、获得SSH账号
这个是首要条件，否则后面的操作都没有意义。但如何获得SSH账号不在本文的表诉范围。

##二、配置PuTTy

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

##三、配置switchysharp

switchysharp是chrome的一个插件，安装后作如下配置：

第一步：配置情景模式(Proxy Profiles)
![](/images/switchysharp1.png)

**注：端口号必须和PuTTy的设置保持一致**

第二步：配置切换规则(Switch Rules)

启用切换规则，使用在线规则列表http://autoproxy-gfwlist.googlecode.com/svn/trunk/gfwlist.txt
![](/images/switchysharp2.png)

###备注
PuTTy的内容来自[利用PuTTY的SSH Tunnels实现安全的代理](http://www.huluboke.com/putty-ssh-tunnels/)
