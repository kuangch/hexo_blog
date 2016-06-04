---
title: 优雅的写代码
date: 2016-06-04 11:36:54
tags:
- 优雅代码
categories: 经验
---

#### <center>*代码精简*</center>

- 尽量减少<font color=red>if else</font>的使用，使得代码跟你家精简优雅（可能降低阅读性）

    <font color='#4590a3'>例如：递归写一个字符串反转的小程序</font>

    常规写法：
```JavaScript
function reverseStr(str){
    if(str){
        return '';
    }else{
        return reverseStr(str.substr(1)) + str.charAt(0);
    }
}
```

    优雅写法：
```JavaScript
function reverseStr(str){
    return str && reverseStr(str.substr(1)) + str[0];
}
```
