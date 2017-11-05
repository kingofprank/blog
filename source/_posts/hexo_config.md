---
title: hexo使用注意事项
date: 2016-08-7 19:07:47
tags: 
---

一些小坑的地方，折腾了半天才莫名其妙的好了。
<!--more-->

# 用hexo s后无法进入127.0.0.1:4000

刚开始装完系统后完全没什么问题，突然有一天就不能进入127.0.0.1:4000了。hexo没有任何的报错提示，就是在浏览器上不能访问，刚开始查了一下以为是IIS出了问题。后来在一篇博客上突然发现，原来有可能是某个进程占用了端口。
解决的的办法也很简单，换一个端口就可以了。
```cpp
hexo s -p 5000
```
这样在浏览器上输入127.0.0.1:5000就可以访问了。不过我们下一步要找出罪魁祸首。
打开命令行，输入命令
```cpp
netstat -ano
```
列出所有端口情况，可以观察被占用端口进程的PID值。然后切换到任务管理器，在进程选项卡里，查看对应PID的服务，结果发现是Foxit Service服务占用了，就是这个坑爹的**福昕阅读器**，导致了无法访问！！