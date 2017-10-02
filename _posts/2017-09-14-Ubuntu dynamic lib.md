---
layout: post
title: Ubuntu dynamic libraies
moto: 最近真是累。。。
date: 2017-09-14 +0800
categories: document
img: DSC01368_.jpg
tag: Ubuntu OpenGL
---

* content
{:toc}

# Ubuntu 动态链接库

这段时间在集成之前写的代码，OpenGL也真是个麻烦的东西。

在生成的vpRecommendation项目中，编译之后程序竟然不能脱离IDE运行！

然后用lld查看了一下依赖库

```
linux-vdso.so.1 =>  (0x00007fff4e181000)
libfftw3f.so.3 => /usr/local/lib/libfftw3f.so.3 (0x00007f27793a6000)
libGLEW.so.1.13 => /usr/lib/x86_64-linux-gnu/libGLEW.so.1.13 (0x00007f2779122000)
libGLU.so.1 => /usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGLU.so.1 (0x00007f2778eb3000)
libassimp.so.3 => /usr/local/lib/libassimp.so.3 (0x00007f2778289000)
libopencv_core.so.3.2 => /usr/local/lib/libopencv_core.so.3.2 (0x00007f2777514000)
libopencv_imgcodecs.so.3.2 => /usr/local/lib/libopencv_imgcodecs.so.3.2 (0x00007f27771b4000)
libopencv_imgproc.so.3.2 => /usr/local/lib/libopencv_imgproc.so.3.2 (0x00007f2775964000)
libopencv_objdetect.so.3.2 => /usr/local/lib/libopencv_objdetect.so.3.2 (0x00007f27756fa000)
libopencv_saliency.so.3.2 => /usr/local/lib/libopencv_saliency.so.3.2 (0x00007f27754c9000)
libQt5Widgets.so.5 => /home/hejw005/Qt5.9.0/5.9/gcc_64/lib/libQt5Widgets.so.5 (0x00007f2774c98000)
libQt5Gui.so.5 => /home/hejw005/Qt5.9.0/5.9/gcc_64/lib/libQt5Gui.so.5 (0x00007f27744ea000)
libQt5Core.so.5 => /home/hejw005/Qt5.9.0/5.9/gcc_64/lib/libQt5Core.so.5 (0x00007f2773db2000)
libGL.so.1 => /usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGL.so.1 (0x00007f2773b0e000)
libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f277378b000)
libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f2773482000)
libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f277326c000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f2772ea1000)
libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x00007f2772c87000)
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f2772a82000)
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f2772865000)
librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f277265d000)
libjpeg.so.8 => /usr/lib/x86_64-linux-gnu/libjpeg.so.8 (0x00007f2772403000)
libwebp.so.5 => /usr/lib/x86_64-linux-gnu/libwebp.so.5 (0x00007f27721a7000)
libpng12.so.0 => /lib/x86_64-linux-gnu/libpng12.so.0 (0x00007f2771f82000)
libtiff.so.5 => /usr/lib/x86_64-linux-gnu/libtiff.so.5 (0x00007f2771d0d000)
libjasper.so.1 => /usr/lib/x86_64-linux-gnu/libjasper.so.1 (0x00007f2771ab8000)
libIlmImf-2_2.so.22 => /usr/lib/x86_64-linux-gnu/libIlmImf-2_2.so.22 (0x00007f27715ea000)
libHalf.so.12 => /usr/lib/x86_64-linux-gnu/libHalf.so.12 (0x00007f27713a6000)
libopencv_highgui.so.3.2 => /usr/local/lib/libopencv_highgui.so.3.2 (0x00007f2771199000)
libicui18n.so.56 => /home/hejw005/Qt5.9.0/5.9/gcc_64/lib/libicui18n.so.56 (0x00007f2770cff000)
libicuuc.so.56 => /home/hejw005/Qt5.9.0/5.9/gcc_64/lib/libicuuc.so.56 (0x00007f2770947000)
libicudata.so.56 => /home/hejw005/Qt5.9.0/5.9/gcc_64/lib/libicudata.so.56 (0x00007f276ef64000)
libgthread-2.0.so.0 => /usr/lib/x86_64-linux-gnu/libgthread-2.0.so.0 (0x00007f276ed61000)
libglib-2.0.so.0 => /lib/x86_64-linux-gnu/libglib-2.0.so.0 (0x00007f276ea50000)
/lib64/ld-linux-x86-64.so.2 (0x000056409c45a000)
libGLX.so.0 => /usr/lib/nvidia-375/libGLX.so.0 (0x00007f276e820000)
libGLdispatch.so.0 => /usr/lib/nvidia-375/libGLdispatch.so.0 (0x00007f276e551000)
liblzma.so.5 => /lib/x86_64-linux-gnu/liblzma.so.5 (0x00007f276e32f000)
libjbig.so.0 => /usr/lib/x86_64-linux-gnu/libjbig.so.0 (0x00007f276e120000)
libIex-2_2.so.12 => /usr/lib/x86_64-linux-gnu/libIex-2_2.so.12 (0x00007f276df02000)
libIlmThread-2_2.so.12 => /usr/lib/x86_64-linux-gnu/libIlmThread-2_2.so.12 (0x00007f276dcfb000)
libgtk-x11-2.0.so.0 => /usr/lib/x86_64-linux-gnu/libgtk-x11-2.0.so.0 (0x00007f276d6af000)
libgdk-x11-2.0.so.0 => /usr/lib/x86_64-linux-gnu/libgdk-x11-2.0.so.0 (0x00007f276d3fa000)
libcairo.so.2 => /usr/lib/x86_64-linux-gnu/libcairo.so.2 (0x00007f276d0e5000)
libgdk_pixbuf-2.0.so.0 => /usr/lib/x86_64-linux-gnu/libgdk_pixbuf-2.0.so.0 (0x00007f276cec3000)
libgobject-2.0.so.0 => /usr/lib/x86_64-linux-gnu/libgobject-2.0.so.0 (0x00007f276cc70000)
libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007f276c9ff000)
libX11.so.6 => /usr/lib/x86_64-linux-gnu/libX11.so.6 (0x00007f276c6c5000)
libXext.so.6 => /usr/lib/x86_64-linux-gnu/libXext.so.6 (0x00007f276c4b3000)
libgmodule-2.0.so.0 => /usr/lib/x86_64-linux-gnu/libgmodule-2.0.so.0 (0x00007f276c2ae000)
libpangocairo-1.0.so.0 => /usr/lib/x86_64-linux-gnu/libpangocairo-1.0.so.0 (0x00007f276c0a1000)
libXfixes.so.3 => /usr/lib/x86_64-linux-gnu/libXfixes.so.3 (0x00007f276be9b000)
libatk-1.0.so.0 => /usr/lib/x86_64-linux-gnu/libatk-1.0.so.0 (0x00007f276bc75000)
libgio-2.0.so.0 => /usr/lib/x86_64-linux-gnu/libgio-2.0.so.0 (0x00007f276b8ed000)
libpangoft2-1.0.so.0 => /usr/lib/x86_64-linux-gnu/libpangoft2-1.0.so.0 (0x00007f276b6d7000)
libpango-1.0.so.0 => /usr/lib/x86_64-linux-gnu/libpango-1.0.so.0 (0x00007f276b48a000)
libfontconfig.so.1 => /usr/lib/x86_64-linux-gnu/libfontconfig.so.1 (0x00007f276b247000)
libXrender.so.1 => /usr/lib/x86_64-linux-gnu/libXrender.so.1 (0x00007f276b03c000)
libXinerama.so.1 => /usr/lib/x86_64-linux-gnu/libXinerama.so.1 (0x00007f276ae39000)
libXi.so.6 => /usr/lib/x86_64-linux-gnu/libXi.so.6 (0x00007f276ac29000)
libXrandr.so.2 => /usr/lib/x86_64-linux-gnu/libXrandr.so.2 (0x00007f276aa1e000)
libXcursor.so.1 => /usr/lib/x86_64-linux-gnu/libXcursor.so.1 (0x00007f276a813000)
libXcomposite.so.1 => /usr/lib/x86_64-linux-gnu/libXcomposite.so.1 (0x00007f276a610000)
libXdamage.so.1 => /usr/lib/x86_64-linux-gnu/libXdamage.so.1 (0x00007f276a40d000)
libpixman-1.so.0 => /usr/lib/x86_64-linux-gnu/libpixman-1.so.0 (0x00007f276a164000)
libfreetype.so.6 => /usr/lib/x86_64-linux-gnu/libfreetype.so.6 (0x00007f2769eba000)
libxcb-shm.so.0 => /usr/lib/x86_64-linux-gnu/libxcb-shm.so.0 (0x00007f2769cb5000)
libxcb-render.so.0 => /usr/lib/x86_64-linux-gnu/libxcb-render.so.0 (0x00007f2769aab000)
libxcb.so.1 => /usr/lib/x86_64-linux-gnu/libxcb.so.1 (0x00007f2769889000)
libffi.so.6 => /usr/lib/x86_64-linux-gnu/libffi.so.6 (0x00007f2769680000)
libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007f276945e000)
libresolv.so.2 => /lib/x86_64-linux-gnu/libresolv.so.2 (0x00007f2769242000)
libharfbuzz.so.0 => /usr/lib/x86_64-linux-gnu/libharfbuzz.so.0 (0x00007f2768fe4000)
libthai.so.0 => /usr/lib/x86_64-linux-gnu/libthai.so.0 (0x00007f2768ddb000)
libexpat.so.1 => /lib/x86_64-linux-gnu/libexpat.so.1 (0x00007f2768bb1000)
libXau.so.6 => /usr/lib/x86_64-linux-gnu/libXau.so.6 (0x00007f27689ad000)
libXdmcp.so.6 => /usr/lib/x86_64-linux-gnu/libXdmcp.so.6 (0x00007f27687a6000)
libgraphite2.so.3 => /usr/lib/x86_64-linux-gnu/libgraphite2.so.3 (0x00007f2768580000)
libdatrie.so.1 => /usr/lib/x86_64-linux-gnu/libdatrie.so.1 (0x00007f2768377000)
```

此外，运行程序会直接崩掉，发现原因是OpenGL的版本不对，但是在Qt IDE中可以看到OpenGL的版本是正确的
```
glwidget initial ... 
OpenGL version is (2.1 Mesa 7.2)
shader language support: 0
Error: 
Error: failed to preprocess the source.
```
通过这个log信息就可以看到我的这个程序根本就没有调用起Nvidia的显卡嘛。。。。

再分析一下上面的ldd结果就发现
```
libGLU.so.1 => /usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGLU.so.1 (0x00007f2778eb3000)
libGL.so.1 => /usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGL.so.1 (0x00007f2773b0e000)
```
这两个这么关键的库，居然用的是matlab里面的

===========

这里面一定还要其他的解决方案，我想一定是可以为可执行程序重新指定动态依赖库来解决的。

预计的解决方案是，重新修改环境变量的顺序。

我的直觉是可执行程序会在环境变量里面去寻找依赖库，找到就不会再找了，当然目前还没有验证。

现在的解决方案是，修改libGLU.so.1和libGL.so.1的软连接。


## 先备份

```
cd /usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/

sudo mv libGLU.so.1 libGLU.so.1.back
sudo mv libGL.so.1 libGL.so.1.back
```

## 建立软连接

```
# 可以通过locate libGLU, locate libGL来确定库的位置

locate libGLU

/usr/lib/x86_64-linux-gnu/libGLU.a
/usr/lib/x86_64-linux-gnu/libGLU.so
/usr/lib/x86_64-linux-gnu/libGLU.so.1
/usr/lib/x86_64-linux-gnu/libGLU.so.1.3.1
/usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGLU.rights
/usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGLU.so.1
/usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGLU.so.1.3.070200

#会发现也发现了MATLAB的库。。。。直接修改软连接

sudo ln -s /usr/lib/x86_64-linux-gnu/libGLU.so.1 libGLU.so.1


locate libGL

/usr/lib/nvidia-375/libGL.so
/usr/lib/nvidia-375/libGL.so.1
/usr/lib/nvidia-375/libGL.so.1.0.0

/usr/lib/x86_64-linux-gnu/libGL.so

/usr/lib/x86_64-linux-gnu/libGLU.a
/usr/lib/x86_64-linux-gnu/libGLU.so
/usr/lib/x86_64-linux-gnu/libGLU.so.1
/usr/lib/x86_64-linux-gnu/libGLU.so.1.3.1

/usr/lib/x86_64-linux-gnu/mesa/libGL.so
/usr/lib/x86_64-linux-gnu/mesa/libGL.so.1
/usr/lib/x86_64-linux-gnu/mesa/libGL.so.1.2.0

/usr/lib32/nvidia-375/libGL.so
/usr/lib32/nvidia-375/libGL.so.1
/usr/lib32/nvidia-375/libGL.so.1.0.0

/usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGL.so.1
/usr/local/MATLAB/R2015b/sys/opengl/lib/glnxa64/libGL.so.1.5.070200

# =.=! 这个libGL居然有这么多库，经过尝试之后发现，使用/usr/lib/nvidia-375/libGL.so.1程序才可以正常运行

sudo ln -s /usr/lib/nvidia-375/libGL.so.1 libGL.so.1

```