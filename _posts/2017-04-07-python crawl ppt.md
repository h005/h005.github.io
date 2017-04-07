---
layout: post
title: Slides crawl with python 
moto: Work hard! Play hard!
date: 2017-04-07 13:55:00 +0800
categories: document
tag: crawl downloader
---

```
# -*- coding: utf-8 -*-
# @Author: hejw005
# @Date:   2017-03-25 09:51:18
# @Last Modified by:   h005
# @Last Modified time: 2017-03-26 09:58:59

import wget
import requests
import bs4
import re

import io

# the website
prefix = 'https://courses.engr.illinois.edu/cs543/sp2015/'
response = requests.get(prefix)

# I want to use this to wirte the html file into file
f = io.open('output.txt','w',encoding='utf-8')

soup = bs4.BeautifulSoup(response.text,"html.parser")

# regular expression to find the string start with 'lectures/' and end with '.pdf'
# ref http://www.runoob.com/regexp/regexp-tutorial.html
pattern = re.compile('lectures/.*\.pdf')

# print soup.find_all(href = pattern)

ind = 0
for ele in soup.find_all(href = pattern):
	print ind
	# print ele(0)
	# get the link
	# ref https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html
	tmpHref = ele.get('href')
	tmpHref = tmpHref.encode("utf-8")
	# print tmpHref
	
	# analysis the html file and split the stirng with '%20-%20'
	pattern2 = re.compile('%20-%20')
	print tmpHref
	lis = tmpHref.split('%20-%20')
	# reorganize the file name
	filename = ''.join(lis[1].split('%20'))
	tmpHref = prefix + tmpHref
	filename = str(ind) + '_' + filename
	# print filename
	wget.download(tmpHref,filename+'.pdf')
	ind = ind + 1;

print 'done'
```

