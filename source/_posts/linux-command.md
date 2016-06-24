---
title: 开发中使用的Linux命令
date: 2016-05-26 19:50:45
tags:
- 命令
categories: Linux
---

#### 常用
``` bash
# 内核版本
cat /proc/version
uname -a
uname -r

# 系统版本
lsb_release -a
cat/etc/issue

# cpu信息
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c

# 内存信息
cat /proc/meminfo

# 硬盘信息
df -h

# 内存，进程占用，
free
top

```
#### 监听一个目录下指定类型文件的个数
例如：查看/xxx/目录下*.jpg文件的个数，并监听(一秒间隔)变化
``` bash
cd /xxx
# -l 表示显示行数（即文件个数）
watch -n 1 "ls *.jpg | wc -l"
```
#### find / ls 高级用法
查看/查找 /xxx/目录下*.jpg文件
``` bash
ls /xxx | grep jpg
find /xxx -iname "*.jpg"
```

#### 动态链接库环境变量配置

  `问题描述:` linux命令行下运行facemoding文件。可以正常执行，但是运行过程中报错。但是通过qt可以运行（猜测可能qt环境依赖库完整，问题可能是缺少依赖库）通过 ` ldd facemoding`查看程序引用的库。发现引用了`/usr/lib`系统默认的OpenNI库,这是由于系统优先寻找这个目录的连接库。我们删除这个库并且把自己的OpenNI库的路径添加到动态链接库环境变量`ld.so.conf.d`中：
  ``` bash
  # openni.conf 名字随便取，在/etc/ld.so.conf.d/目录下的配置文件配置的路径都在系统搜索范围
  vim /etc/ld.so.conf.d/openni.conf
  # 编辑输入："path of ypur library"
  ```

#### 防火墙打开80端口
刚安装好nginx无法站外访问，本机wget、telnet都正常。而服务器之外，不管是局域网的其它主机还是互联网的主机都无法访问站点。
如果是以上的故障现象，很可能是被CentOS的防火墙把80端口拦住了，尝试执行以下命令，打开80端口：
``` bash
iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```

