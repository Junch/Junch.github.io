---
layout: post
title: "常用svn命令"
summary: ""
description: ""
tags: ["svn", ""]
categories: ["笔记"]
---

#### 1.取代码
```bash
svn checkout/co https://projectn/svn/trunk/ projectname --username username@gmail.com

svn update/up -rXXXX
svn update/up .
```

#### 2.提交代码
```bash
svn commit -m "added howto section."
svn commit -F msg.txt .
```

#### 3.撤销提交
```bash
svn merge -c -1944 .
svn revert filename
```

#### 4.查看
```bash
svn log -r 236144 -v
svn log -r {2016-05-03}:{2016-05-04} | sed -n '1p; 2,/^-/d; /huiluo/,/^-/p'
svn log --verbose --search huiluo --revision {2016-05-01}:{2016-05-05}
svn log --search juchen3 -l 500 https://wwwin-svn-gpk.cisco.com/jabber-all/jabber/trunk

svn status | findstr "^M" # Find modified files on windows
svn status | grep ^M
```

#### 5.添加删除移动
```bash
svn add thegeekstuff
svn delete thegeekstuff
svn move src dest
```

#### 6.合并
```bash
svn merge -c svn merge -c 254669 https://wwwin-svn-gpk.cisco.com/jabber-all/jabber/trunk
```

### 参考

- [svn cheat sheet 1](http://www.cheat-sheets.org/saved-copy/subversion-cheat-sheet-v1.pdf)
- [svn cheat sheet 2](https://deveo.com/svn-commands)
- [7 Subversion SVN Merge Command Examples for Branch and Trunks](http://www.thegeekstuff.com/2014/03/svn-merge-command)