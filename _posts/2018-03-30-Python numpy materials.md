---
layout: post
title: Python Numpy Materials
moto: If you are still looking for that one person who will change your life, take a look in the mirror.
date: 2018-03-30 +0800
categories: document
tag: Python Numpy
---


<table border="0" align="center">
<tr>
<td align="center">
<img src="/assets/18spring/DSC6618_.jpg" width="384" height="216"/>
</td>
<td align="center">
<img src="/assets/18spring/DSC6622_.jpg" width="384" height="216"/>
</td>
</tr>
<tr>
<td align="center">
<img src="/assets/18spring/DSC6625_.jpg" width="384" height="216"/>
</td>
<td align="center">
<img src="/assets/18spring/DSC6627_.jpg" width="384" height="216"/>
</td>
</tr>
</table>


## Python Numpy


[https://docs.scipy.org/doc/numpy-dev/user/quickstart.html](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html)

[python-numpy-tutorial of cs231n](http://cs231n.github.io/python-numpy-tutorial/)

[python class](https://docs.python.org/3.5/tutorial/classes.html)

[python numpy for matlab users](http://scipy.github.io/old-wiki/pages/NumPy_for_Matlab_Users)

[python numpy of broadcasting](https://docs.scipy.org/doc/numpy/user/basics.broadcasting.html)

[python array broadcasting in numpy](http://scipy.github.io/old-wiki/pages/EricsBroadcastingDoc)


## Matlab Startup Error: no write permission

```
user@user-pc:/usr/local/MATLAB/R2017b/bin$ ./matlab

Fatal Internal Error: Unexpected exception: 'N14cmddistributor16RuntimeExceptionE: Internal Error: No write permission on directory /home/hejw005/.matlab/R2017b/temp0x23d30x281b. Details: fl:filesystem:AccessDenied.' in createMVMAndCallParser phase 'Creating local MVM'
```

This can happen if the *~/.matlab* directory is owned by root. Make yourself owner of the directory:

```
sudo chown -R $USER: ~/.matlab
```

ref [https://askubuntu.com/questions/844535/matlab-startup-error-no-write-permission](https://askubuntu.com/questions/844535/matlab-startup-error-no-write-permission)


## git socks5 proxy settings

```
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```