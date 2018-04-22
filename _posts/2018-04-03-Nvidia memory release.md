---
layout: post
title: Nvidia Memory Release
moto: 以出世之心，做入世之事
date: 2018-04-03 +0800
categories: document
tag: Deep Learning
---

* content
{:toc}

## NVIDIA GPU Memory Release

GPU 的显存是被占了，使用nvidia-smi命令又没有找到占用显存的进程ID

![screenshot1](/assets/nvidiaMemory/nm1.jpg)

可使用如下命令查看进程id
```
sudo fuser -v /dev/nvidia*
```
![screenshot1](/assets/nvidiaMemory/nm2.jpg)

kill these process
![screenshot1](/assets/nvidiaMemory/nm3.png)

ref [https://stackoverflow.com/questions/15197286/how-can-i-flush-gpu-memory-using-cuda-physical-reset-is-unavailable](https://stackoverflow.com/questions/15197286/how-can-i-flush-gpu-memory-using-cuda-physical-reset-is-unavailable)

