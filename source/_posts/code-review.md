---
title: 发布code review
date: 2016-05-28 14:18:28
tags:
- review
categories: 工具
---

### 准备条件
- Git 和 Review Board 关联
- review 和 Bugzilla 关联

### 生成diff文件
> 明确起始commit 然后生成一个基于这两个commit之间的diff文件，首先查看提交信息获取起始commit的hash值如下：

    $ git log
    commit dd483a4f44a3d962d9cd886108c5aea7239b483c
    Author: gitlab <gitlab@loongmian.com>
    Date:   Sat May 28 11:25:34 2016 +0800

    test2

    commit 1fd3aa6996d395accb5fa633200a87895c45c9f3
    Author: gitlab <gitlab@loongmian.com>
    Date:   Sat May 28 11:24:54 2016 +0800

    test

我们选生成`test`到`test2`之间的diff文件使用如下命令

    # ./diff.diff 为自己自定的diff保存位置
    # 1fd3aa 和 dd483分别为test 和test2 连个commit对应的hash值
    $ git diff 1fd3aa..dd483 > ./diff.diff

这样，在当前目录上一级目录下将生成一个名为diff.diff文件

### 提交Bug信息

> 我们生成了test 和 test2 连个commit之间的diff文件，这个两者之间改动的代码做了什么？ 完成了哪些功能？ 修复了哪些bug？ 需要记录到bugzilla中。加入我们的 test 和 test2之间是增加了一个“打电话”的功能。那么我们需要做如下操作

 - 打开 bugzill 主页（192.168.1.94）

 ![](/imgs/code_review/1.jpg)

 - 选择所属项目（这里假如我们做的是 TestProduct项目）

 ![](/imgs/code_review/2.jpg)

 - 提交 bug，提交bug后会得到这个bug的bugId 如下：

 ![](/imgs/code_review/3.jpg)

### 提交code review

> 经过以上两部以后 我们现在又一个diff文件，又一个 Bug ID，有了这些我们就可以发布code review了。具体流程如下：

- 打开review board （192.168.1.96:8081）

 ![](/imgs/code_review/4.jpg)

按上图顺序操作（由于演示，review board没有TestProject项目，我随便选的，真实情况下只要保证选择的是同一个项目就行）然后选择我们第一步生成的diff文件，如下图：

 ![](/imgs/code_review/5.jpg)

点击打开后将会跳转到如下界面：

 ![](/imgs/code_review/6.jpg)

图中标红框为必点内容，线面分别对这些内容简单说明：

- **概要**：描述这次需要比人review的代码做了哪些事情，做个简要描述，注意概述的格式为`[项目名称][版本号][分支名] 概要内容`
- **详细说明**：概要描述不足以体现更多的内容。详细说明可以详细介绍
- **单元测试说明**：填写单元测试的覆盖率，通过率..如果没有单元测试请说明原因，或简要说明
- **分支名**：通过code review 后要提交到仓库的分支，一般为`master`
- **Bug ID**：这次代码变化，提交的bug的 bug Id
- **reviewer**：指定你需要对你代码进行code review的人

确保所有的信息无误点击绿色框标出的ok 按钮，最后点击publish按钮完成code review的提交