---
title: 如何 clone git 项目到一个非空目录
date: 2016-07-17 23:33:16
tags: git
---

如果我们往一个非空的目录下 clone git 项目，就会提示错误信息：

fatal: destination path '.' already exists and is not an empty directory.

解决的办法是：

``` bash
$ cd git
$ git clone --no-checkout https://git.oschina.net/NextApp/blog.git tmp
$ mv tmp/.git .   #将 tmp 目录下的 .git 目录移到当前目录
$ rmdir tmp
$ git reset --hard HEAD
```
然后就可以进行各种正常操作了。