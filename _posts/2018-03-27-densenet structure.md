---
layout: post
title: Densenet in Pytorch
moto: 让未来的自己感谢你今天所做的一切
date: 2018-03-27 +0800
categories: document
tag: deep learning
img: 1565847305.jpg
---

* content
{:toc}

## pytorch densenet

pytorch densenet is constructed with the class of _DenseBlock

we can judge the dense block with the code below:
```
isinstance(layer,models.densenet._DenseBlock)

```

the stucture of the densenet is:
```
Conv2d id = 0
BatchNorm2d id = 1
ReLU id = 2
MaxPool2d id = 3
_DenseBlock id = 4
	_DenseLayer x 6
		BatchNorm2d
		ReLU
		Conv2d
		BatchNorm2d
		ReLU
		Conv2d
_Transition id = 5
	BatchNorm2d
	ReLU
	Conv2d
	AvgPool2d
_DenseBlock id = 6
	_DenseLayer x 12
		BatchNorm2d
		ReLU
		Conv2d
		BatchNorm2d
		ReLU
		Conv2d
_Transition id = 7
	BatchNorm2d
	ReLU
	Conv2d
	AvgPool2d
_DenseBlock id = 8
	_DenseLayer x 36
		BatchNorm2d
		ReLU
		Conv2d
		BatchNorm2d
		ReLU
		Conv2d
_Transition id = 9
	BatchNorm2d
	ReLU
	Conv2d
	AvgPool2d
_DenseBlock id = 10
	_DenseLayer x 24
		BatchNorm2d
		ReLU
		Conv2d
		BatchNorm2d
		ReLU
		Conv2d
_BatchNorm2d id = 11

```

## Call for Another Graphics Card with Pycharm

Run -> Edit Configutrations -> Environment variables

set CUDA_VISIBLE_DEVICES as 0, 1, or other values

## Class and Instance in Python

ref [https://www.cnblogs.com/Lambda721/p/6130209.html](https://www.cnblogs.com/Lambda721/p/6130209.html)

## matlab laplacian filtering

## conference list

IEEE International Conference on Multimedia & Expo 2017

ACM SIGMM International Conference on Multimedia Retrieval Jan. 30th, 2018

Eurographics Conference on Visualization 2017

Computer Animation and Social Agents Jan 27, 2018

Computer Graphics International February 13, 2018, 23:59 UTC

IEEE Pacific Visualization Symposium 2017

C International Conference on Multimedia Modeling Jul 15, 2018

C Pacific-Rim Conference on Multimedia May 5, 2018 

C ACCV Asian Conference on Computer Vision Jul 5, 2018

C British Machine Vision Conference 30 April 2018

## matlab Semantic Segmentation Using Deep Learning

[https://cn.mathworks.com/help/vision/examples/semantic-segmentation-using-deep-learning.html](https://cn.mathworks.com/help/vision/examples/semantic-segmentation-using-deep-learning.html)