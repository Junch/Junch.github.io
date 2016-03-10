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

NPM有类似的问题，也有必要使用淘宝镜像

{% highlight bash lineno %}
$ npm config set registry https://registry.npm.taobao.org

# 配置后可通过下面方式来验证是否成功
$ npm config get registry
# 或者
$ npm info express
{% endhighlight %}

### 补充

今天(2016/3/10)将jekyll从3.1.1退回到3.0.3, 一方面和GitHub Pages的版本保持一致，参考[Dependency versions](https://pages.github.com/versions/)。另一方面3.1.1中有一个bug，
[Jekyll 3.1.1 doesn't support GFM fenced codeblocks](https://github.com/udevbe/udevbe.github.io/issues/3)，虽然这个bug已经在3.1.2中修掉了。

值得记录的几件事情：

- 使用gem命令在source中搜索jekyll的所有版本，使用正则表达式，否则会搜索到太多的gems。

```bash
$ gem search ^jekyll$ -a
```

- 使用GFM fenced codeblocks后，就可以放弃liquid notation。以前为了语法高亮，需要使用右边的语法：`{{"{% highlight ruby "}}%} . . . {{"{% endhighlight "}}%}`，和markdown非常不搭调。

- 从jekyll 3.0开始，强制只能使用kramdown做markdown解释。kramdown和一般的markdown差异参看[kramdown和markdown较大的差异比较](http://platinhom.github.io/2015/11/06/Kramdown-note/)。

- 从jekyll 3.0开始，强制只能使用rouge做语法解释(原来是pygments)。

### 参考
- [国内优秀npm镜像推荐及使用](http://riny.net/2014/cnpm/)
- [Github升级Jekyll3.0-强制使用rouge语法高亮](Github升级Jekyll3.0-强制使用rouge语法高亮)
- [Upgrading to Jekyll 3.0](http://kersulis.github.io/2015/10/31/jekyll-3/)
