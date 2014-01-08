---
layout: post
title: "在Sublime Text 2 中使用Ctags实现CoffeeScript 函数跳转"
description: ""
category: ""
tags: []
comments: true
---


关于Ctags就不多介绍了，直接进入正文吧


### 安装Sublime Text 2 CTags插件
[https://github.com/SublimeText/CTags](https://github.com/SublimeText/CTags)

###在Mac下使用Ctags问题
[http://gmarik.info/blog/2010/10/08/ctags-on-OSX](http://gmarik.info/blog/2010/10/08/ctags-on-OSX)

###使CoffeeScript支持Ctags
 - 修改~/.ctags  文件
 >[https://gist.github.com/yury/2624883](https://gist.github.com/yury/2624883)
 - 使用CoffeeTags来生成.tag文件
 >[https://github.com/lukaszkorecki/CoffeeTags](https://github.com/lukaszkorecki/CoffeeTags)

但是这个对Ctag的支持并不好，@变量无法实现到跳转,剩下的再补充