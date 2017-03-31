---
layout: post
title:  ubuntu 16.04 3d deep learning 
moto:	你眼中看似落叶纷飞变化无常的世界，实际只是躺在上帝怀中一份早已谱好的乐章。
date:   2017-03-31 10:00:00 +0800
categories: document
tag: Ubuntu
---

## Ubuntu 16.04 meshlab install

suggest you to this site [http://linuxg.net/how-to-install-meshlab-1-3-3-on-ubuntu-linux-mint-and-elementary-os-via-ppa/](http://linuxg.net/how-to-install-meshlab-1-3-3-on-ubuntu-linux-mint-and-elementary-os-via-ppa/)

>	$ sudo add-apt-repository ppa:zarquon42/meshlab

>	$ sudo apt-get update

>	$ sudo apt-get install meshlab

to remove meshlab

>	$ sudo apt-get remove meshlab

Or, if you want to uninstall meshlab, disable the recently added PPA and downgrade all the packages that got updated via the PPA, do:

>	$ sudo apt-get install ppa-purge

>	$ sudo ppa-purge ppa:zarquon42/meshlab


##  Ubuntu 16.04 cuda-convent2 install

suggest you to this site [https://github.com/akrizhevsky/cuda-convnet2](https://github.com/akrizhevsky/cuda-convnet2)

switch to the wiki branch and read the Compiling.md file

>	sudo apt-get install python-dev python-numpy python-scipy python-magic python-matplotlib libatlas-base-dev libjpeg-dev libopencv-dev git

But I think that the git and libopencv-dev are not necessary to install


Then compile the lib with the command:

>	sh build.sh

 
