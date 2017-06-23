---
layout: post
title: Matlab save figure without padding
moto: Work hard! Play hard!
date: 2017-05-14 11:25:56
categories: document
tag: Ubuntu
---

* content
{:toc}


Before step into the context of saving figures in matlab, I found a good website for sublime plugins to insert the time.

ref [https://my.oschina.net/antsky/blog/491146](https://my.oschina.net/antsky/blog/491146)

My **add_date.py** file was modified as

```
import datetime, getpass
import sublime, sublime_plugin

class AddDateTimeStampCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        self.view.run_command("insert_snippet",
            {
                # "contents": "%s" % datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S %A")
                # 可根据自己的需要进行调整（参照后面的日期时间格式）
                "contents": 
                "%s"  %datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            }
        )
```

## Save Figure without padding

<table border="2" align="center">
<tr>
<td align="center">
<img src="/assets/matlab/car_0001_padding.png" width="320" height="240"/>
</td>
<td align="center">
<img src="/assets/matlab/car_0001.png" width="320" height="240"/>
</td>
</tr>
<tr>
<td align="center">
heat map with padding
</td>
<td align="center">
heat map without padding
</td>
</tr>
</table>


In this case, I have a matrix constructing an image, but I want to save its heat map, but not the
image self. But if we save the heat map figure with the function of **saveas()**, we will get an image
with padding. And I want get the heat map without the padding with the following steps:

### Show the Heat Map

To show the heat map, we should map the image data first.
```
image(img,'CDataMapping','scaled')
colormap jet
```

### Remove the axis, and save the figure
	
```
axis off
saveas(fig,savePath);
```

### Re-read the figure and remove the padding

```
R = imread(savePath);
% get the gray image
Rgray = rgb2gray(R);
% get the sum of each col and each row
Rcol = sum(Rgray);
Rrow = sum(Rgray,2);
% compute the gradients between every two cols and every rows
RrowGrad = abs(Rrow(2:end) - Rrow(1:end-1));
% sort the gradients, the most two larget values corresponding to the padding edge
[RrowGrad,RrowIndex] = sort(RrowGrad,'descend');
RcolGrad = abs(Rcol(2:end) - Rcol(1:end-1));
[RcolGrad,RcolIndex] = sort(RcolGrad,'descend');

rowSize = [RrowIndex(1),RrowIndex(2)];
colSize = [RcolIndex(1),RcolIndex(2)];
% resort the rowSize and colSize to extract the heap map image
rowSize = sort(rowSize);
colSize = sort(colSize);

R = R(rowSize(1):rowSize(2),colSize(1):colSize(2),:);
```


### Source code:
```
%% this script for save the image without padding
function saveImgWithoutPadding(img, imgName)
fig = figure;
image(img,'CDataMapping','scaled')
colormap jet


axis off
% save the figure first
savePath = imgName;
saveas(fig,savePath);
R = imread(savePath);
Rgray = rgb2gray(R);
Rcol = sum(Rgray);
Rrow = sum(Rgray,2);
RrowGrad = abs(Rrow(2:end) - Rrow(1:end-1));
[RrowGrad,RrowIndex] = sort(RrowGrad,'descend');

RcolGrad = abs(Rcol(2:end) - Rcol(1:end-1));
[RcolGrad,RcolIndex] = sort(RcolGrad,'descend');

rowSize = [RrowIndex(1),RrowIndex(2)];
colSize = [RcolIndex(1),RcolIndex(2)];
rowSize = sort(rowSize);
colSize = sort(colSize);

R = R(rowSize(1):rowSize(2),colSize(1):colSize(2),:);

imwrite(R,savePath);

close all

disp(['save to ' savePath])
```



