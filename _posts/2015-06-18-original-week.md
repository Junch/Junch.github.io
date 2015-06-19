---
layout: post
title: "一周回顾 (1)"
tagline: ""
description: ""
tags: ["Centos", "mongodb", "node.js"]
categories: ["笔记"]
---
这周主要做了下面一些事情

1. 在CentOS7上安装MongoDB和Node.js
2. 使用Node.js, Express和MongoDB创建REST API
3. 熟悉CentOS的基本命令
4. 配置Azure上的CentOS7服务器

安装MongoDB和Node.js是参考网页[How To Install MongoDB On CentOS 7](http://www.unixmen.com/install-mongodb-centos-7/)和[How To Install Node.js On CentOS 7](http://www.unixmen.com/install-node-js-centos-7/)。

创建REST API是参考网页[Creating a REST API using Node.js, Express, and MongoDB](http://coenraets.org/blog/2012/10/creating-a-rest-api-using-node-js-express-and-mongodb/)。由于版本更新，源代码也需要更新才行。mongodb模块的api可以参考[MongoDB 2.0.0 Driver](http://mongodb.github.io/node-mongodb-native/2.0/)。

在itpub上下载了视频Udemy - Learning Linux CentOS From Scratch，同步到ipad上，准备在上下班路上看。

使用ssh连上Azure上的服务器，安装上MongoDB和Node.js，的确方便。

在CentOS虚拟机上安装MongoDB和Node.js，源代码依然放在宿主机上，通过共享目录访问。这种工作模式和文章[Docker 笔记 打造node.js开发环境 安装nvm](https://www.ijser.cn/install-nvm-on-docker/)介绍的模式有异曲同工之妙。思路是好，但在客户机上执行命令npm install的时候出现问题（宿主机是windows）。共享目录不支持symlink。解决方案是加上参数--no-bin-links，可以参考文档[Install a package](https://docs.npmjs.com/cli/install)。
