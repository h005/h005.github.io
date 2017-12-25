---
layout: post
title: Pytorch models
moto: 17年就要过去了
date: 2017-12-25 +0800
categories: document
tag: Pytorch vgg
---

* content
{:toc}

## 使用pytorch提取已训练好的网络的特征

ref [https://github.com/Atcold/pytorch-CortexNet/blob/master/notebook/network_bisection.ipynb](https://github.com/Atcold/pytorch-CortexNet/blob/master/notebook/network_bisection.ipynb)

ref [https://discuss.pytorch.org/t/how-to-extract-features-of-an-image-from-a-trained-model/119/3](https://discuss.pytorch.org/t/how-to-extract-features-of-an-image-from-a-trained-model/119/3)

ref [https://discuss.pytorch.org/t/how-to-return-the-features-of-vgg-fc1-layer/6145](https://discuss.pytorch.org/t/how-to-return-the-features-of-vgg-fc1-layer/6145)

ref [https://discuss.pytorch.org/t/how-to-modify-the-final-fc-layer-based-on-the-torch-model/766/6](https://discuss.pytorch.org/t/how-to-modify-the-final-fc-layer-based-on-the-torch-model/766/6)

```
from __future__ import print_function
import torch
from torch.autograd import Variable
from PIL import Image
import matplotlib.pyplot as plt
import torchvision.transforms as transforms
import torchvision.models as models


use_cuda = torch.cuda.is_available()
dtype = torch.cuda.FloatTensor if use_cuda else torch.FloatTensor

# 在此处导入models.vgg19_bn(pretrained=True).features和models.vgg19_bn(pretrained=True).classifier以及models.vgg19_bn(pretrained=True)得到的网络是不一样的，当导入.features会得到除全连接层之外的网络层，而.classifier则会得到全连接层，什么都不加则会得到所有的网络层。
# cnn = models.vgg19_bn(pretrained=True).features
cnn = models.vgg19_bn(pretrained=True).classifier

# move it to the GPU if possible:
if use_cuda:
    cnn = cnn.cuda()

# desired size of the output image
imsize = 224

loader = transforms.Compose([
    transforms.Scale(imsize),  # scale imported image
    transforms.ToTensor()])  # transform it into a torch tensor

def image_loader(image_name):
    image = Image.open(image_name)
    image = Variable(loader(image))
    # print image.data.size
    # fake batch dimension required to fit network's input dimensions
    image = image.unsqueeze(0)
    return image

style_img = image_loader("images/picasso.jpg").type(dtype)

# 截取到模型的最后一个4096的全连接层
myClassifier = nn.Sequential(*list(cnn.classifier.children())[:-3])
# 替换模型的classifier层
cnn.classifier = myClassifier
# 输入图像，提取特征
target_feature = cnn(style_img).clone()
```
