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