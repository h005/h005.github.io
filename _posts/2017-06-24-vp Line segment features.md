---
layout: post
title: vpSegmentDetection Project
moto: 道不行，乘桴浮于海。从我者，其由与？
date: 2017-06-24 21:18:30 +0800
categories: document
tag: paper
---

毕业季,今天实验室同级的同学也离开了..

---

视点推荐需要在加入一些关于建筑物的2D特征,我目前想到的办法是用计算消失点检测出来的
线段来描述.

目前用来描述的有

* 线段与X轴的夹角直方图

* 该直方图的方差

* 该直方图的熵

因为目前计算消失点时,只有3个消失点,也就是说line segment有3类,如图所示:

{% include image.html url="/assets/vpRecommendation/lineSegment.png" description="Fig. 1. line segmentation result." %}
