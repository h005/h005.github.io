---
layout: post
title: Nvidia Driver Install in Ubuntu 16.04
moto: 以出世之心，做入世之事
date: 2020-04-29 +0800
categories: document
tag: Ubuntu
---

* content
{:toc}

两年多没有更了。。。

最近不小心又把Ubuntu系统的nvidia驱动搞崩了，什么登陆界面循环，nvidia-smi找不到设备等等，为此已经重装了好几次系统了，现总结一下解决方案

### 已有nvidia驱动

这个时候一定要先卸载，具体命令如下：


```

sudo apt-get remove --purge nvidia-*

sudo apt-get autoremove

sudo apt-get install -f

sudo reboot

```

如果有如下报错：
```
X Error of failed request: BadValue (integer parameter out of range for operation)
```
则

```

sudo apt-get purge nvidia*

sudo apt-get install -reinstall xserver-xorg-video-intel libgl1-mesa-glx libgl1-mesa-dri xserver-xorg-core

sudo dpkg-reconfigure xserver-xorg

```
重新安装NVIDIA驱动，进入fft1

```

sudo service lightdm stop

sudo ./cuda_10.1.168_418.67_linux.run --no-opengl-libs (若有独立显卡一定要加此参数)

sudo service lightdm start
```
就可以进入了

### 无nvidia驱动

一定要禁用nouveau

```

sudo vim /etc/modprobe.d/blacklist.conf

在文本的最后添加如下内容

blacklist nouveau
option nouveau modeset=0

保存退出

sudo update-initramfs -u

重启电脑，查看是否生效

lsmod | grep nouveau

```

然后按照

```

sudo service lightdm stop

sudo ./cuda_10.1.168_418.67_linux.run --no-opengl-libs (若有独立显卡一定要加此参数)

sudo service lightdm start

```

安装即可
