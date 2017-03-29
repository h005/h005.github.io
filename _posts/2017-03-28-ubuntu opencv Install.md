---
layout: post
title:  ubuntu 16.04 opencv install
date:   2017-03-28 12:08:00 +0800
categories: document
tag: Ubuntu
---

* content
{:toc}

## Install opencv 3.2 and opencv contrib on Ubuntu 16.04

先给个非常实用的链接[http://milq.github.io/install-opencv-ubuntu-debian/](http://milq.github.io/install-opencv-ubuntu-debian/)

运行install-opencv.sh

>	install-opencv.sh 该脚本基本上讲安装opencv所需要的库都装全了，但是具体要安装什么库还是可以修改这个文件的。


```
\# KEEP UBUNTU OR DEBIAN UP TO DATE

sudo apt-get -y update

sudo apt-get -y upgrade

sudo apt-get -y dist-upgrade

sudo apt-get -y autoremove


\# INSTALL THE DEPENDENCIES

\# Build tools:

sudo apt-get install -y build-essential cmake

\# GUI (if you want to use GTK instead of Qt, replace 'qt5-default' with 'libgtkglext1-dev' and remove '-DWITH_QT=ON' option in CMake):

sudo apt-get install -y qt5-default libvtk6-dev

\# Media I/O:

sudo apt-get install -y zlib1g-dev libjpeg-dev libwebp-dev libpng-dev libtiff5-dev libjasper-dev libopenexr-dev libgdal-dev

\# Video I/O:

sudo apt-get install -y libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev yasm libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev libxine2-dev

\# Parallelism and linear algebra libraries:

sudo apt-get install -y libtbb-dev libeigen3-dev

\# Python:

sudo apt-get install -y python-dev python-tk python-numpy python3-dev python3-tk python3-numpy

\# Java:

sudo apt-get install -y ant default-jdk

\# Documentation:

sudo apt-get install -y doxygen

\# INSTALL THE LIBRARY (YOU CAN CHANGE '3.2.0' FOR THE LAST STABLE VERSION)

sudo apt-get install -y unzip wget

wget https://github.com/opencv/opencv/archive/3.2.0.zip

unzip 3.2.0.zip

rm 3.2.0.zip

mv opencv-3.2.0 OpenCV

cd OpenCV

mkdir build

cd build

cmake -DWITH_QT=ON -DWITH_OPENGL=ON -DFORCE_VTK=ON -DWITH_TBB=ON -DWITH_GDAL=ON -DWITH_XINE=ON -DBUILD_EXAMPLES=ON ..

make -j4

sudo make install

sudo ldconfig


\# EXECUTE SOME OPENCV EXAMPLES AND COMPILE A DEMONSTRATION

\# To complete this step, please visit 'http://milq.github.io/install-opencv-ubuntu-debian'.
```


安装java8，我比较喜欢通过ppa来安装，简单粗暴，还不用设置环境变量

```
sudo add-apt-repository ppa:webupd8team/java

sudo apt-get update

sudo apt-get install oracle-java8-installer 

开个terminal，输入java，javac可以看一下java是否已经就绪

卸载jdk

sudo apt-get remove oracle-java8-installer
```

##### 至此开始搞定opencv以及opencv contrib

>	一定要注意，从源码编译的时候不要直接从git上clone opencv的最新代码，建议使用正式发布的代码，我这里实用的是opencv (3.2.0) + opencv_contrib (3.2.0)，要不然会出现很奇葩的错误。

由于我最想安装的是opencv的sfm模块，所以特别关注了这部分内容[http://docs.opencv.org/trunk/db/db8/tutorial_sfm_installation.html](http://docs.opencv.org/trunk/db/db8/tutorial_sfm_installation.html)

>	要安装的库，参考上述网页即可，但是据我的经验发现，直接实用apt安装是有问题的。当使用cmake编译的时候会报找不到头文件的的错，进而导致opencv sfm模块安装不成功。
所以建议从源码编译依赖的各个库。

实用cmake-gui编译下载好的opencv源代码

在第一次Configure之后的OPENCV_EXTRA_MODULES_PATH变量中填好opencv contrib/modules

再次configure然后generage

然后执行make -jn //n 处理器的是线程数

###	done


