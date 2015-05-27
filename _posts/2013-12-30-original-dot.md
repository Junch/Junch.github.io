---
layout: post
title: "使用Graphviz绘制UML类图"
tagline: "程序员的好工具"
description: ""
tags: ["Graphviz", "dot", "uml"]
categories: ["笔记"]
---
###安装
{% highlight bash %}
$ brew install graphviz
{% endhighlight %}

如果需要安装PyGraphviz，可以执行

{% highlight bash %}
$ easy_install pygraphviz
{% endhighlight %}

###一个简单的类图

编写一个简单的文本文件如下所示：

{% highlight text %}
digraph G {   
    node[shape=record
         fontname = "Bitstream Vera Sans"
         fontsize = 10
         style = "filled"
         color = "black"
         fillcolor = "#fafad2"
    ]

    edge[shape=record
         fontname = "Bitstream Vera Sans"
         fontsize = 8
    ]
   
    TTTAppDelegate[label="{TTTAppDelegate|-array : NSMutableArray\l-game : Game\l|-observeValueForKeyPath()\l}"]
    Game[label="{Game||-setChess()\l-circleResponse()\l}"]
    NSMutableArray[label="{NSMutableArray|}"]
    TTTBoard[label="{TTTBoard|-array : NSMutableArray\l|-drawRect()\l-mouseDown()\l}"];

    {rank=same;TTTAppDelegate;Game}
    {rank=max;NSMutableArray;TTTBoard}

    edge[ arrowhead="diamond" taillabel="1"]
    Game -> TTTAppDelegate;
    NSMutableArray ->TTTAppDelegate;

    TTTBoard -> NSMutableArray [arrowhead="vee" taillabel="" constraint=false];
}
{% endhighlight %}

假设将文件存盘保存为TTT.dot，执行命令

{% highlight bash %}
$ dot -T png TTT.dot -o TTT.png
{% endhighlight %}

生成的类图如下所示：

![](/images/dot-TTT.png)

###关于dir属性

绘制中发现dir属性需要拿出来特别强调一下，在[文档][]中对dir有这样的说明：

{% highlight text %}
Set edge type for drawing arrowheads. This indicates which ends of the edge should be decorated with an arrowhead. The actual style of the arrowhead can be specified using the arrowhead and arrowtail attributes.
{% endhighlight %}

简单写一个例子：

{% highlight text %}
digraph G {  
	A -> B [arrowhead="vee"]
	AA -> BB [dir="back" arrowtail="vee"]
	AAA -> BBB [dir="both" arrowhead="vee" arrowtail="odiamond"]
}
{% endhighlight %}

这个dot脚本生成的图片如下

![](/images/dot-dir.png)

###参考
- [UML Diagrams Using Graphviz Dot](http://www.ffnn.nl/pages/articles/media/uml-diagrams-using-graphviz-dot.php)
- [UML class diagrams in Confluence using Graphviz and DOT](http://blog.lunatech.com/2007/04/27/uml-class-diagrams-confluence-using-graphviz-and-dot)
- [yUML](http://www.yuml.me)
- [Gliffy](http://www.gliffy.com)

[文档]: http://www.graphviz.org/doc/info/attrs.html
