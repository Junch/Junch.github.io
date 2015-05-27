---
layout: post
title: "安装Jekyll"
summary: 更换gem源，否则会被墙
tagline: ""
description: ""
tags: ["Jekyll"]
categories: ["技巧"]
---
现在忽然有了写点什么的冲动，准备继续启用自己的一亩三分地。安装Jekyll时卡壳了，原来是gem源被墙了，需要把gem源换成淘宝的源。

{% highlight bash lineno %}
$ gem sources --remove https://rubygems.org/  
$ gem sources -a http://ruby.taobao.org/  
$ gem sources -l 
# 请确保只有 ruby.taobao.org，如果不移除rubygems.org，安装会非常慢。
{% endhighlight %}
