---
title: git 简易的命令行入门教程
date: 2018-03-12 10:36:53
tags:git 
---

简易的命令行入门教程:

Git 全局设置:
```
git config --global user.name "caohui"
git config --global user.email "firstcaohui@gmail.com"
```

创建 git 仓库:

```
mkdir ad_plat
cd ad_plat
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/caohui/ad_plat.git
git push -u origin master
```

已有项目?

```
cd existing_git_repo
git remote add origin https://gitee.com/caohui/ad_plat.git
git push -u origin master
```