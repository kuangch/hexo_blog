---
title: 真实工作中Git提交流程
date: 2016-05-26 21:39:14
tags:
- git
categories: 工具
---
## 说明
关于git的使用方法，命令什么的，这些在网上的资料一大堆，没有必要赘述。我只就真实的公司开发环境下的使用写出我的使用经验。（我公司老板冲Vmware带出来的经验之谈，比较规范）

## 流程
- 在code-review结束后我们才能提交代码（code-review后续会讲）
- 本地master分支更新远端git最新代码

``` bash
git checkout master
git status
# 当本地master分支落后于远端master分支时，执行下列语句
git pull
git checkout your_working_branch
git rebase master
```

- 将your_working_branch压缩为一个commit

``` bash
git log  # 查看log数目
git rebase -i HEAD~合并的数目
HEAD_Former..HEAD_Later
```

- 给合成的commit写注释

``` bash
 <summary>
 Description:
     xxxx
 Testing Done:
     xxxx
 Reviewed By: kuangch, sunhe
 Approved By: kuangch, sunhe
 Bug Number: xxxx
 Review Board ID: xxxx
```

- 将your_working_branch合并到主分支上

``` bash
 git checkout master
 git merge your_working_branch
```

- 将代码提交


``` bash
git push
```

## 例子
- "ship-it" 恭喜你code-review被虐结束

``` bash
git checkout master
git status
```

- 查看远端分支是否有新的提交，如果有新的提交，则执行下列代码更新


``` bash
git pull
git checkout kuangch
git rebase master
```

- 将sunhe分支的commit合为1个


``` bash
git log

# 例如如下所示，有两个提交
# hash1:659ea22XXX
# second
# hash2:78c0e12XXX
# third

git rebase -i HEAD~2

# 会显示如下两条记录
# pick 659ea22 second
# pick 78c0e12 third
# 将底下的pick改为s
# wq即会切换到comment界面

#加入如下评论（包括，提交简述，修改描述，测试信息，代码审阅人，bugid，reviewBoardID）
 adding upload_client.py and recv_server.py to implement transportation between local and center

 Description:
     It is a transportation module between local and center.
     1. When RGBD source that have been compressed is prepared,
     upload_client.py will send it to the center. At the same time, info of
     collector will be send as well.
     2. recv_server.py will receive all the data client send, and update
     all the info to the center database.

 Testing Done:
     there are test_server.py and test_client.py
     please run:
     1.nosetests -s --with-coverage test_server.py &
     2.nosetests -s --with-coverage test_client.py

     test result:
     All test case pass

     coverage result:
     Name                               Stmts   Miss  Cover
     transform_constant                    31      1    97%
     upload_client                         97     24    75%
     recv_server                           85     36    58%

 Reviewed By: hulei, kuangch
 Approved By: hulei, kuangch
 Bug Number: 2
 Review Board ID: 4
```

- 将your_working_branch合并到主分支上


``` bash
git checkout master
git merge kuangch
```

- 将代码提交到远端

``` bash
git push
```

