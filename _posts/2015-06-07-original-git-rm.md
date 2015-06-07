---
layout: post
title: "Git: 使用rm命令"
tagline: ""
description: ""
tags: ["git", ""]
categories: ["技巧"]
---

在jekyll中文件夹.sass-cache下的文件没必要提交到depot，就在.gitignore中添加了一句

{% highlight text %}
.sass-cache/
{% endhighlight %}

结果发现仅仅这样做是不够的，还需要将以前提交的文件删除。

{% highlight bash %}
git rm -r .sass-cache
{% endhighlight %}

详细的解释可参考文档[git help rm](http://linux.die.net/man/1/git-rm)。

另外可参考网页[git忽略已经被提交的文件](http://segmentfault.com/q/1010000000430426)。