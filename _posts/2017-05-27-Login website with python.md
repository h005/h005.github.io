---
layout: post
title: Log in website with python.
moto: Work hard! Play hard!
date: 2017-05-27 16:09:35 +0800
categories: document
tag: Ubuntu
---

* content
{:toc}

To log in a website, we need submit some parameters to it, and the website will make some responses. 

There are two HTTP Request methods: GET and POST.

## POST

Submits data to be processed to a specified resource

## GET

Requests data from a specified resource


## Comparison

| | GET | POST |
----|----|----|
| BACK button/Reload | Harmless	| Data will be re-submitted (the browser should alert the user that the data are about to be re-submitted) |
| Bookmarked | Can be bookmarked | Cannot be bookmarked |
| Encoding type	| application/x-www-form-urlencoded | application/x-www-form-urlencoded or multipart/form-data. Use multipart encoding for binary data |
| Cached | Can be cache	| Not cached |
| History | Parameters remain in browser history | Parameters are not saved in browser history |
| Restrictions on data length | Yes, when sending data, the GET method adds the data to the URL; and the length of a URL is limited (maximum URL length is 2048 characters) | No restrictions |
| Restrictions on data type | Only ASCII characters allowed | No restrictions. Binary data is also allowed |
| Security | GET is less secure compared to POST because data sent is part of the URL. Never use GET when sending passwords or other sensitive information! | POST is a little safer than GET because the parameters are not stored in browser history or in web server logs |
| Visibility | Data is visible to everyone in the URL | Data is not displayed in the URL |

## Log in with urllib and urllib2

We need to prepare the parameter in the function of urlopen(url, data, timeout). It is necessary to prepare a dict with the keywords of 'username' and 'password' (we should check the source code of the website to check the keywords).

```
import urllib
import urllib2
values = {"username":"1016903103@qq.com","password":"XXXX"}
data = urllib.urlencode(values) 
url = "https://passport.csdn.net/account/login?from=http://my.csdn.net/my/mycsdn"
request = urllib2.Request(url,data)
response = urllib2.urlopen(request)
print response.read()  
```

To log in the website of [p.nju.edu.cn](p.nju.edu.cn)

just modify the url to [http://p.nju.edu.cn/portal_io/login](http://p.nju.edu.cn/portal_io/login) is ok.

But it is unsafe to write the username and the password into the script directly. So I suggest to receive the input from the console. And then, the code with modification is shown below.

```
import urllib
import urllib2
import getpass

# read in the username
username = raw_input('username:')

# read in the password without display
password = getpass.getpass()


values = {}
values['username'] = username
values['password'] = password

data = urllib.urlencode(values)

url = "http://p.nju.edu.cn/portal_io/login"

request = urllib2.Request(url,data)
reponse = urllib2.urlopen(request)

context = reponse.read()

msg = context.split(',')

# print the login state
for ele in  msg:
    if ele.split(':')[0] == '"reply_msg"':
        print ele.split(':')[1]
        break
```
