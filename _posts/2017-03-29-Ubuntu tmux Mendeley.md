---
layout: post
title: ubuntu 16.04 tmux and Mendeley
date: 2017-03-29 21:13 +0800
categories: document
tag: Ubuntu
---

* content
{:toc}

## tmux and Mendeley

### install tmux

>	sudo apt-get install tmux

### install Mendeley

download the deb file [https://www.mendeley.com/download-mendeley-desktop/ubuntu/instructions/](https://www.mendeley.com/download-mendeley-desktop/ubuntu/instructions/)

>	sudo dpkg -i <path-to-downloaded-package>

#### Mendeley proxy setup

Mendeley cannot synchronize because some reasons. To solve it, we can set the proxy.

Here is the method to set sock5 agent.

> tools->options->Connection

> socks 5 proxy, server: 127.0.0.1 port: 1080

 
