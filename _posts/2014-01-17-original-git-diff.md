---
layout: post
title: "Git: 使用DiffMerge作为diff/merge工具"
tagline: ""
description: ""
tags: ["git"]
categories: ["技巧"]
---
{% include JB/setup %}

SourceGear公司的DiffMerge非常好用，而且免费。将其设置为git的diff/merge工具也不复杂。首先下载PKG Installer安装。如果使用DMG安装的话，还需要另外安装命令行工具。接着执行下面的命令：

{% highlight bash %}
$ git config --global diff.tool diffmerge
$ git config --global difftool.diffmerge.cmd
    "/usr/bin/diffmerge \"\$LOCAL\" \"\$REMOTE\""

$ git config --global merge.tool diffmerge
$ git config --global mergetool.diffmerge.trustExitCode true
$ git config --global mergetool.diffmerge.cmd 
    "/usr/bin/diffmerge --merge --result=\"\$MERGED\"
        \"\$LOCAL\" \"\$BASE\" \"\$REMOTE\""
{% endhighlight %}

为了避免每次运行git difftool都有提示信息，可以输入如下命令
{% highlight bash %}
$ git config --global difftool.prompt false
{% endhighlight %}

这些--global的信息都存放在 ~/.gitconfig 文件中。必要的时候(比如输入数据错误)，你可以手工编辑。

在Mac下还可以设置FileMerge作为diff工具，可以参考文章[HOWTO: Using FileMerge (opendiff) with Git on OSX](https://gist.github.com/bkeating/329690)。
