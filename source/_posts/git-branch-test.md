---
title: 在Git仓库搭建测试环境
date: 2016-05-26 21:50:35
tags:
- git
categories: 工具
---

#### 开发中遇到的实际问题
> 由于公司有严格的代码管理制度（需要master分支保持洁净，代码必须review通过才能提交等等），而我们的测试环境都在远程Linux主机上通过又不能通过copy代码进行测试（实践证明copy是非常愚蠢的，可能造成代码混乱，搞不清楚版本，难以回退维护等等问题）开发需要进行，但是规范不能丢啊。所以想了个办法：我们在远端仓库建立一个远程test分支，每个开发者往test分支推送，远程Linux主机pull代码切换到test分支使用test分支代码进行测试。
ok，let's go!

#### 操作步骤

*基于本地`master`创建远程`test`分支*

``` bash
# 新建一个和本地master一样的远程test分支
git push origin master:test
```

*在本地拉取远程`test`分支并创建本地`test`分支*

``` bash
# 先执行git fetch, 从github上取回新的信息放到本地仓库中
git fetch
# 基于origin/test在本地创建test分支
git checkout -b test origin/test
```

现在每个开发者可以test分支上任意合并工作分支的代码了

#### 注意事项

*不要在test分支上提交代码*

>  由于我们在`test`分支多人协作，如果直接在test分支提交代码，功能完成后要合并到`master`分支上，我们把`test`合并到`master`最后的结果是别人的提交历史是你提交的，假如你们在`test`上工作的时候产生冲突合并，这些不好的历史将带到`master`后果很严重。
   正确的做法是在工作分支提交你的代码，让后merge到`test`分支，现在`test`分支就是所有协作者的代码

*如果工作分支rebase了怎么办*

> 如果我们的工作分支rebase了，可能合并了commit，交换了commit的顺序等，这个时候在向`test`合并可能产生大量冲突，这个时候最好删除远程`test`分支，再重新建立一个

``` bash
# 删除本地的远程test分支记录
git branch -r -d origin/test
# 把空分支推送到远程test分支（相当于删除分支）
git push origin :test
```
然后重复 **操作步骤**