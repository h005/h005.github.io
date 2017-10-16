---
layout: post
title:  ubuntu 16.04 sublime text3 package
date:   2017-03-28 16:08:00 +0800
categories: document
tag: Ubuntu
---

* content
{:toc}

## sublime text3 package

## 首先配置package control

依此网站做即可[https://packagecontrol.io/installation](https://packagecontrol.io/installation)

一些重要的插件cq

>	SideBarEnhancements

>	Alignment

>	PackageResourceViewer

>	SyncedSidebarBg 侧边栏主题和主窗口同步

>	SublimeCodeIntel 实现语法自动完成功能

>	AutoPEP8 自动将Python规范化

>	SublimeREPL 

>	Python Breakpoints

## Ubuntu 16.04 sublime 无法输入中文解决方案

参考[http://www.jianshu.com/p/bf05fb3a4709](http://www.jianshu.com/p/bf05fb3a4709)去解决的

强烈建议使用上文中的第一条办法去解决，最为简单

[https://github.com/lyfeyaj/sublime-text-imfix](https://github.com/lyfeyaj/sublime-text-imfix)


## sublime 快捷键绑定

```
[
	{ "keys": ["ctrl+i"], "command": "move", "args": {"by": "characters", "forward": true} },
	{ "keys": ["ctrl+u"], "command": "move", "args": {"by": "characters", "forward": false} },
	{ "keys": ["ctrl+j"], "command": "move", "args": {"by": "lines", "forward": true} },
	{ "keys": ["ctrl+k"], "command": "move", "args": {"by": "lines", "forward": false} },
	{ "keys": ["ctrl+d","ctrl+t"], "command": "add_date_time_stamp" }
]
```
