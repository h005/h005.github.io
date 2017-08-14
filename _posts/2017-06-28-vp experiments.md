---
layout: post
title: Viewpoint recommendation experiments analysis
moto: Play hard, work hard!
date: 2017-06-28 19:22:48 +0800
categories: document
tag: paper
---

## 不同方法对比:

目前使用的2D特征有:

* ruleOfThird
* hogHist
* LineSegment
* vanish Line
* gist

3D特征没有动

在加入了新的特征之后,总的错误率比之前又下降了,效果是好了的.

{% include image.html url="/assets/vpRecommendation/classificationResults.jpg" description="Fig. 1. classification results." %}

Elapsed time is 397.676964 seconds.

## 视点推荐热力图

不同的模型推荐出来的视点结果也不一样,整体结果的效果好坏还不好下定论.但是目前,北大楼推荐出的热力图中推荐点明显偏向于左侧.
经过分析之后主要是由GIST特征导致的,**GIST特征对于对称的两张照片也不能得到对称的特征**.

GIST特征是一种场景的特征描述.有如下五个方面:

* 自然度（Degree of Natura你lness）：场景如果包含高度的水平和垂直线，这表明该场景有明显的人工痕迹，通常自然景象具有纹理区域和起伏的轮廓。所以，边缘具有高度垂直于水平倾向的自然度低，反之自然度高。

* 开放度（Degree of Openness）：空间包络是否是封闭（或围绕）的。封闭的，例如：森林、山、城市中心。或者是广阔的，开放的，例如：海岸、高速公路。
    
* 粗糙度（Degree of Roughness）：主要指主要构成成分的颗粒大小。这取决于每个空间中元素的尺寸，他们构建更加复杂的元素的可能性，以及构建的元素之间的结构关系等等。粗糙度与场景的分形维度有关，所以可以叫复杂度。
    
* 膨胀度（Degree of Expansion）：平行线收敛，给出了空间梯度的深度特点。例如平面视图中的建筑物，具有低膨胀度。相反，非常长的街道则具有高膨胀度。
    
* 险峻度（Degree of Ruggedness）：即相对于水平线的偏移。（例如，平坦的水平地面上的山地景观与陡峭的地面）。险峻的环境下在图片中生产倾斜的轮廓，并隐藏了地平线线。大多数的人造环境建立了平坦地面。因此，险峻的环境大多是自然的。

在实际求解中,一个图像被划分成了4 x 4的区域,然后每个区域得到了一个20维的直方图,将三个通道的直方图拼接到一起形成GIST特征.因此GIST特征不是对称的.

## 点云模型的精细程度与视点推荐的关系

就目前的实验来看,基本上恢复出整个模型,点云模型的精细程度还是有影响的,模型越精细越好.

当然,这个跟采样点的数量也有关系,稀疏的那个点云为了计算速度快一些只采样了256个视点,而稠密的采样了1024个点,回去再计算一下.

<table border="2" align="center">
<tr>
<td align="center">
<img src="/assets/vpRecommendation/njuSample.jpg" width="320" height="240"/>
</td>
<td align="center">
<img src="/assets/vpRecommendation/njuSample2.jpg" width="320" height="240"/>
</td>
</tr>
<tr>
<td align="center">
dense pt model
</td>
<td align="center">
sparse pt model
</td>
</tr>
</table>