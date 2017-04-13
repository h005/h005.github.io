---
layout: post
title: Slides crawl with python 
moto: Work hard! Play hard!
date: 2017-04-07 13:55:00 +0800
categories: document
tag: crawl downloader
---

##	Codes for crawl slides with python

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

## Character encoding problem with crawl

I have crawl data from [https://www.baidu.com](https://www.baidu.com) and save the request's text into a log file, but there exists an encoding error.

```
import requests
r = requests.get('https://www.baidu.com')
print r.text
```

save the the text info to the logIn.log file

```
python logIn.py > logIn.log
```

The error info:


```
Traceback (most recent call last):
  File "logIn.py", line 11, in <module>
    print r.text
UnicodeEncodeError: 'ascii' codec can't encode characters in position 317-343: ordinal not in range(128)
```

Then, I found that this is caused by the error encoding problem, so I tried to solve this problem by```
 writing the text info into a file with the encoding of 'utf-8'

Here is the code:


```
import requests
import io
f = io.open('logIn.log','w',encoding='utf-8')
r = requests.get('https://www.baidu.com')
f.write(r.text)
f.close()
```

The output file is still full of messy codes.

At last, I found that the web's encoding is not the same as the file's encoding. And we can use **r.encoding** to check the encoding of the web text.

The code is:


```
import requests
import io
f = io.open('logIn.log','w',encoding='ISO-8859-1')
r = requests.get('https://www.baidu.com')
print r.encoding
f.write(r.text)
f.close()

```

By print the r.encoding, we can find that its encoding is 'ISO-8859-1'. After saving the text info to the file with this encoding, everything goes well .
