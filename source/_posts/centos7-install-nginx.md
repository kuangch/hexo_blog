---
title: 在centOS7中安装nginx防火墙设置问题
date: 2016-06-26 19:50:45
tags:
- nginx
- centOS7
-
categories: Linux
---

#### 问题描述
在centOS7安装nginx后，不能访问nginx。由于防火墙关闭了80端口。

#### <font color=red>解决方式一</font>
``` bash
iptables -A INPUT -p tcp --dport 21 -j ACCEPT
```
重启系统后配置还原了，80端口依然墙了，尝试修改配置文件

``` bash
vi /etc/sysconfig/iptables
```
发现根本没有这个配置文件。后来发现centOS7默认使用了Firewall

#### <font color=red>解决方式二</font>
``` bash
# 查看已开放端口：
firewall-cmd --zone=public --list-all

# 添加端口（2048为端口号）：
firewall-cmd --zone=public --add-port=80/tcp --permanent

# reload生效：
firewall-cmd --reload
```

重启后配置依然在，可以访问80端口


