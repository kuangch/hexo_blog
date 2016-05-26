---
title: 在Git仓库搭建测试环境
date: 2016-05-26 21:50:35
tags:
- git
categories: 工具
---

#### 开发中遇到的实际问题

    由于公司有严格的代码管理制度（需要master分支保持洁净，代码必须review通过才能提交等等），而我们的测试环境都在远程Linux主机上通过又不能通过copy代码进行测试（实践证明copy是非常愚蠢的，可能造成代码混乱，搞不清楚版本，难以回退维护等等问题）开发需要进行，但是规范不能丢啊。所以想了个办法：我们在远端仓库建立一个远程test分支，每个开发者往test分支推送，远程Linux主机pull代码切换到test分支使用test分支代码进行测试。
    ok，let's go!

#### 操作步骤

1. 基于本地`master`创建远程`test`分支
``` bash
# 新建一个和本地master一样的远程test分支
git push origin master:test
```
2. 在本地拉取远程`test`分支并创建本地`test`分支
``` bash
# 先执行git fetch, 从github上取回新的信息放到本地仓库中
# 基于origin/test在本地创建test分支
git checkout -b test origin/test
```