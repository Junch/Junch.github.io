---
layout: post
title: "在Mac10.10上安装Pandoc"
summary: "安装Pandoc并将markdown转换成pdf文件"
description: ""
tags: ["pandoc", "markdown"]
categories: ["笔记"]
---

我已经习惯使用Markdown记笔记和写博客。但有一种情况，使用Markdown并不好使，那就是写比较正式的技术文档。这些文档通常用word, powerpoint编写，便于交流。Markdown不是所见即所得，这妨碍了使用它直接交流。

Pandoc的出现很好的解决了Markdown的这个疼点。他可以把Markdown转换成pdf文档，而pdf是一个比word,powerpoint更通用的格式。这一点和程序员写的源代码非常相似，给客户的不会是源代码，而是编译链接后的可执行文件。

这里主要介绍如何在Mac10.10上安装Pandoc及相关软件。

```bash
brew cask install basictex
brew cask install pandoc

sudo ln -s /usr/texbin/pdflatex /usr/local/bin/
sudo ln -s /usr/texbin/xelatex /usr/local/bin/
```

打印中文会遇到问题，需要安装两个Tex包并下载一个Tex模版。

```bash
sudo /usr/texbin/tlmgr update --self
sudo /usr/texbin/tlmgr install titling
sudo /usr/texbin/tlmgr install lastpage
```

在tzengyuxio的开源项目中下载 [pm-template.latex](https://github.com/tzengyuxio/pages/blob/gh-pages/pandoc/pm-template.latex) 中文模板，将该模板中 setCJKmainfont 字段值 LiHei Pro 修改为电脑上带有的中文字体。

在命令行中执行如下命令

```bash
pandoc --latex-engine=xelatex --template=pm-template doc.md -o doc.pdf
```

Pandoc对markdown也有一些扩展，比如pandoc_title_block, 详细内容参考[Pandoc README](http://pandoc.org/README.html)。

## 参考
- [pandoc转换中文pdf攻略](http://liumh.com/2014/07/18/pandoc-convert-chinese-pdf/)
- [pandoc中文pdf转换攻略](http://afoo.me/posts/2013-07-10-how-to-transform-chinese-pdf-with-pandoc.html)