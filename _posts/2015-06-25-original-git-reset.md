---
layout: post
title: "Git: 撤销上次的提交"
summary: git reset HEAD~1
tagline: ""
description: ""
tags: ["git"]
categories: ["技巧"]
---
{% include JB/setup %}


提交代码后，发现有个文件还没更新好。接下来，要么修改文件，再次提交；要么撤销刚才的提交，改好后再提交。

撤销是执行下面的命令：

{% highlight bash %}
$ git reset HEAD~1
{% endhighlight %}

运行git status，上次提交的文件又回到unstaged的状态。



