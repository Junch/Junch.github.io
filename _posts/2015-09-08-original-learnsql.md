---
layout: post
title: "学习SQL"
summary: ""
description: ""
tags: ["SQL"]
categories: ["笔记"]
---

两个多月一晃而过，博客居然一篇没写。并不是没有学习新东西，相反工作生活学习都比较忙碌。加上时间安排不够合理，博客就耽误了。就从这两天学习的SQL开始吧，把耽误的部分慢慢都补上。


###1. 安装Oracle

首先安装Oracle Database Express Edition 11g Release，这是个免费版本。安装过程中，你会被要求设置SYSTEM用户的密码， 这里假设密码设置为password。安装完毕，可以执行下面的命令来测试。

{% highlight bash %}
SQL> connect system/password
Connected.
{% endhighlight %}

如果一切正常的话，可以在下一行看到反馈消息***connected***。

###2. Unlock sample schema HR
执行下面的命令unlock schema HR.

{% highlight bash %}
SQL> ALTER USER hr ACCOUNT UNLOCK IDENTIFIED BY hr;

User altered.
{% endhighlight %}

执行上面的命令时，注意行尾的**分号**不能省略，否则命令不会执行。接着可以以用户hr来登陆oracle.

{% highlight bash %}
SQL> connect hr/hr
Connected.
{% endhighlight %}


###3. 安装SQL Developer

下载地址: [http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html](http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html)

这是Oracle免费提供的图形工作界面，可以查询数据库，浏览对象，执行脚本等。
