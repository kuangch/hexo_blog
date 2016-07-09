---
title: Android appcompat-v7包引发的错误
date: 2015-12-26 21:37:25
tags:
- appcompat-v7
categories: android
---

### 环境：
Android studio

### 背景：
> 我在向我的工程里面导入一个module的时候，报如下错误：Error:(2) Error retrieving parent for item: No resource found that matches the given name 'android:Widget.Material.Spinner.Underlined'.

### 原因：
我的项目用到的apache的网络请求，android6.0不支持。我将其编译版本设置成了android4.4.。使用的appcompatv7包是23.1.1比较新。导入module的时候不知道怎么回事就导致gradle一直报错。

### 解决：
误打误撞，发现吧工程的编译版本改回android6.0 gradle一下，没报appcompatv7包相关的错误，但是apache的http相关的报错，在次将工程的编译版本改回android4.4
gradle一下，神奇的都不报错了，不知道这是怎么回事。

### 2016-1-15日更新

> 可以通过使用apache的库来规避android6.0不支持apache网络框架的问题。在module的build.gradle的android{}中加入 useLibrary 'org.apache.http.legacy'

   ![](/imgs/android/android_error_appcompatv7.png)

