---
title: Injet IDE 使用技巧
date: 2016-7-9 21:37:25
tags:
- injet
categories: 工具
---

### 正则表达式批量替换
> 我们在项目开发中经常遇到这样的情况，比如我以前是写java的，现在开始写python，大家知道在java中访问一个hashMap对象中的键值，我们都是用obj.key来获取，而在python中字典，不支持这种写法.而作为java老司机的我，在写python程序的时候很容易就写成如下：

![](/imgs/injet/ide_0.jpg)

这时，我们发现要去一个个手动改简直不是人做的事情，喜欢偷懒的为自然回去寻找偷懒的办法，
想到以前学过正则表达式，如果发现cfg_data.dns_server,cfg_data.susppkg_ip, 要把他们都替换成cfg_data['dns_server']这样的形式，观察发现只需要把.dns_server,变成['dns_server'],所以取出. 和 ，之间的内容插入到['...']中间，即可，用正则表达式如此简单：
``` bash
# 匹配所有 cfg.data.dns_server,类型的字符串
cfg_data\.(.*?),
# 替换成 cfg.data['dns_server']类型的字符串
cfg_data[$1],
```
![](/imgs/injet/ide_1.jpg)

### 大功告成:
![](/imgs/injet/ide_2.jpg)