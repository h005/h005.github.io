---
layout: post
title:  ubuntu generate dynamic libs
date:   2017-03-28 13:08:00 +0800
categories: document
tag: Ubuntu
---

* content
{:toc}

# ubuntu 16.04 下生成动态链接库 .so file

在 linux 下，库文件一般放在/usr/lib和/lib下， 

静态库的名字一般为libxxxx.a，其中 xxxx 是该lib的名称；

动态库的名字一般为libxxxx.so.major.minor，xxxx 是该lib的名称，major是主版本号，minor是副版本号

在本文中，我写了一个极简单累加函数：

-	accplus.h
-	accplus.cpp
-	main.cpp // just used for test the my lib

>	accplus.h

``` c++
#ifndef __ACCPLUS_HEADER__
#define __ACCPLUS_HEADER__
#include <iostream>
#include <vector>

void acc();
void acc(std::vector<int> nums);

#endif
```

>	accplus.cpp

``` c++
#include "accplus.h"

void acc()
{
	int res = 0;
	for(int i=1;i<101;i++)
		res += i;
	std::cout << res << std::endl;
}

void acc(std::vector<int> nums)
{
	int len = nums.size();
	int res = 0;
	for(int i=0;i<len;i++)
		res += nums[i];
	std::cout << res << std::endl;
}
```

>	main.cpp

``` c++
#include <iostream>
#include "accplus.h"


// using namespace std;

int main(){
	
	acc();

	int array[] = {1,2,4,2,3,4,123};
	std::vector<int> nums(array,array+sizeof(array)/sizeof(int));
	acc(nums);
    return 0;
}
```

将accplus.cpp编译成libaccplus.so文件
> g++ g++ accplus.cpp -fPIC -shared -o libaccplus.so

> -shared： 该选项指定生成动态连接库		

> -fPIC：表示编译为位置独立(地址无关)的代码，不用此选项的话，编译后的代码是位置相关的，所以动态载入时，是通过代码拷贝的方式来满足不同进程的需要，而不能达到真正代码段共享的目的


将main.cpp编译成可执行文件

>	g++ main.cpp -o main -L. -laccplus

但是运行时却会发现

>	./main: error while loading shared libraries: libaccplus.so: cannot open shared object file: No such file or directory

执行ldd命令会发现

```
h005@h005-pc:~/Documents/learning/gdbLearning/libGen$ ldd main
	linux-vdso.so.1 =>  (0x00007ffefc3a4000)
	libaccplus.so => not found
	libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f9829efb000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f9829b31000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f9829828000)
	/lib64/ld-linux-x86-64.so.2 (0x0000556462854000)
	libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f9829612000)
```

程序根本就没找到libaccplus.so这个库

这是因为环境变量LD_LIBRARY_PATH指定动态库搜索路径，它指定程序动态链接库文件搜索路径，但是却没有包含当前的目录

所以需要进一步的修改LD_LIBRARY_PATH

>	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:.

然后执行ldd main

```
h005@h005-pc:~/Documents/learning/gdbLearning/libGen$ ldd main
	linux-vdso.so.1 =>  (0x00007ffe639ed000)
	libaccplus.so => not found
	libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007febc00c4000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007febbfcfa000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007febbf9f1000)
	/lib64/ld-linux-x86-64.so.2 (0x0000555db6bd8000)
```

然后运行./main，便可看到结果

```
5050
139
```