---
layout: post
title: "Git: 代码冲突常见解决方法"
tagline: "转载"
description: ""
tags: ["git"]
categories: ["技巧"]
---
[原文1](http://blog.csdn.net/iefreer/article/details/7679631)
[原文2](http://www.shanhh.com/blog/2013/01/30/git_FAQ/)

如果系统中有一些配置文件在服务器上做了配置修改,然后后续开发又新添加一些配置项的时候, 在发布这个配置文件的时候,会发生代码冲突:

```text
error: Your local changes to the following files would be overwritten by merge:
        protected/config/main.php
Please, commit your changes or stash them before you can merge.
```

如果希望保留生产服务器上所做的改动,仅仅并入新配置项, 处理方法如下:

```bash
$ git stash
$ git pull
$ git stash pop
```

然后可以使用git diff -w +文件名 来确认代码自动合并的情况.

反过来,如果希望用代码库中的文件完全覆盖本地工作版本. 方法如下:

```bash
$ git reset --hard
$ git pull
```

其中git reset是针对版本,如果想针对文件回退本地修改,使用

```bash
$ git checkout HEAD file/to/restore
```

在 checkout 或者 rebase 时, 如果提示:

```
Please move or remove them before you can switch branches.
Aborting
```

执行:

```bash
$ git clean -d -fx
```

有时 push 代码的时候, 出现提示:

```bash
$ git push
To ../remote/  
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to '../remote/'
```

问题 (Non-fast-forward) 的出现原因在于: git remote 仓库中已经有一部分代码, 所以它不允许你直接把你的代码覆盖上去. 于是你有 2 个选择方式:

- 强推, 即利用强覆盖方式用你本地的代码替代 git 仓库内的内容

```bash
$ git push -f
```

- 或者先把 git 的东西 fetch 到你本地然后 merge 后再 push

```bash
$ git fetch
$ git merge
```