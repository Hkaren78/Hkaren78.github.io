---
layout:     post
title:      "【CSP C++】2019-03-1 小中大"
subtitle:   ""
date:       2019-10-29
author:     "HK"
header-img: "img/miniprogram.jpg"
tags:
    - CSP
---

> 这题虽然很简单，但一定要仔细啊~

## 题目描述

![题目](https://img-blog.csdnimg.cn/20191028175511980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

![题目](https://img-blog.csdnimg.cn/20191029163216438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

## 题意分析

输入：==有序整数==列（不用遍历整个数组，只需比较前面和后面的数值大小,即可得最大值和最小值，根据输入的n是奇数还是偶数来采用不同的方法求中位数）

n<=10^5,输入整数数量不超过100000个（注意申请合适的数组空间）

输出：大中小（不要弄错顺序）

## 错误示例

### 20分

用%.1lf 实现 保留一位小数。

> mid=(a[n/2-1]+a[n/2])/2;

a[n/2-1]+a[n/2]为整数，除2之后还是整数，然后再转化为浮点型赋给mid，故在除2后得到的只是整数部分传给mid，并没有将完整的结果传过去。因此应该先将a[n/2-1]+a[n/2]传给mid,再讲mid除2。
```c
#include<iostream>
#include<iomanip>

int main() {
	int n, min,max;      
 	double mid;
       	int a[100000];     
   	scanf("%d",&n);
	for(int i=0;i<n;i++)
   	{
      		scanf("%d",&a[i]);
  	}      
	if(a[0]>a[n-1])
   	{
      		min=a[n-1];
      		max=a[0];      
	}      
	else
   	{
      		min=a[0];
      		max=a[n-1];
	}   
   	if(n%2==0)     
	{
      		mid=(a[n/2-1]+a[n/2])/2;
      		pintf("%d %.1lf %d",max,mid,min);
	}           
	else      
	{
      		mid=a[n/2];
      		printf("%d %.lf %d",max,mid,min);      
	}     
	return 0; 
}
```

### 70分

没有判断当n为偶数时，输出中位数为整数的情况。

```c
#include<iostream>
#include<iomanip>

int main() {
	int n, min,max;
       	double mid;
       	int a[100000];     
	scanf("%d",&n);
	for(int i=0;i<n;i++)
	{
		scanf("%d",&a[i]);
	}
       if(a[0]>a[n-1])
	{
		min=a[n-1];
		max=a[0];
       }
       else
	{
		min=a[0];
		max=a[n-1];
       }
       if(n%2==0)
       {
		mid=a[n/2-1]+a[n/2];
		mid/=2;
		pintf("%d %.1lf %d",max,mid,min);//没有判断当n为偶数时，输出中位数为整数的情况
       }      
       else
       {
		mid=a[n/2];
		printf("%d %.lf %d",max,mid,min);
       }
       return 0; 
}
```
## 100分（太不容易了）

```c
#include<iostream>
#include<iomanip>
 
int main() {
       int n, min,max;
       double mid;
       int a[100000];
       scanf("%d",&n);
       for(int i=0;i<n;i++){
              scanf("%d",&a[i]);
       }
       if(a[0]>a[n-1]){
              min=a[n-1];
              max=a[0];
       }
       else{
              min=a[0];
              max=a[n-1];
       }
       if(n%2==0){
              mid=a[n/2-1]+a[n/2];
              mid/=2;
              if(mid-(int)mid==0)
		printf("%d %d %d",max,(int)mid,min);
              else
		printf("%d %.1lf %d",max,mid,min);
       }      
       else
       {
              mid=a[n/2];
              printf("%d %d %d",max,(int)mid,min);
       }
              return 0; 
}
```
##  优秀范例分析

>我写的有点太繁琐了，在网上看了一些其他博主写的，感觉很好，跟大家分享下~

### 示例一

> [参考博文](https://blog.csdn.net/richenyunqi/article/details/89185035)

```c
#include<bits/stdc++.h>
using namespace std;

int main(){   
	int n,a;   
	vector<int>v;   //构造一个int型的动态数组
	scanf("%d",&n);   
	for(int i=0;i<n;++i){       
		scanf("%d",&a);       
		if(i==0)           
		v.push_back(a);//       
		if((n%2==1&&i==n/2)||(n%2==0&&i==n/2-1))           
		v.push_back(a);      
		if(n%2==0&&i==n/2)           
		v.push_back(a);       
		if(i==n-1)           
		v.push_back(a);
    	}   
	sort(v.begin(),v.end());//顺序排列数组，sort 需要头文件 #include <algorithm>   
	printf("%d ",v.back());//输出容器中最后一个数，即最大值   
	if(v.size()==3)       
	printf("%d ",v[1]);   
	else if((v[1]+v[2])%2==0)       
	printf("%d ",(v[1]+v[2])/2); 
	else       
	printf("%.1f ",(v[1]*1.0+v[2])/2);   
	printf("%d",v.front());//输出容器中第一个数，即最小值  
	return 0;
}
```
#### 头文件<bits/stdc++.h>

首先看一下他的头文件<bits/stdc++.h>，是一个C++的万能头文件，几乎包含了C++中所有的头文件，像vector,set,string那些函数都可以直接用，以后大家要是嫌麻烦的话就可以用这个头文件啦。

#### vector

>[参考](https://www.runoob.com/w3cnote/cpp-vector-container-analysis.html)     

> vector我平时不是很常用，就去查了一下详细的用法。

**向量（Vector)** 是一个封装了动态大小数组的 **顺序容器** （Sequence Container）。跟任意其它类型容器一样，它能够存放各种类型的对象。可以简单的认为，向量是一个能够存放任意类型的动态数组。

容器特性：

1.顺序序列：顺序容器中的元素按照严格的线性顺序排序。可以通过元素在序列中的位置访问对应的元素。

2.动态数组：支持对序列中的任意元素进行快速直接访问，甚至可以通过指针算述进行该操作。操供了在序列末尾相对快速地添加/删除元素的操作。

3.能够感知内存分配器的（Allocator-aware）：容器使用一个内存分配器对象来动态地处理它的存储需求。

>此题中要求的输入的是有序数组，故可以用vector存放数据，并且vector能够很好地帮助我们动态分配内存，就不用提前申请内存空间了。

一些函数：

void push_back(const T& x)       向量尾部增加一个元素X

int size() const                 返回向量中元素的个数

begin                            得到数组头的指针 

end                              得到数组的最后一个单元+1的指针

front        					           得到数组头的引用 

back             				         得到数组的最后一个单元的引用

### 示例二 

> [参考博文](https://blog.csdn.net/weixin_43074474/article/details/101121750)

```c
#include<iostream>6
#include<iomanip>
using namespace std;

int main() {

       int a[100000];
       int n;
       cin >> n;
       for(int i = 0; i < n; i++) {
              cin >> a[i];
       }
       int min, max;
       int e = 0;
       float mid;
       min = a[0];
       max = a[n - 1];
       if(min > max) {//交换min和max的值
              min = min ^ max;//将a[0]、a[n - 1]按位异或
              max = min ^ max;
              min = min ^ max;
       }
       if(n % 2 == 0) {
              if((a[n / 2 - 1] + a[n / 2]) % 2 != 0)   e= 1;
              mid= ((float)a[n / 2 - 1] + (float)a[n / 2]) / 2;
       }
       else
              mid= a[n / 2];
	cout<< max << " " <<setiosflags(ios_base::fixed)<< setprecision(e)<< mid << " " << min;
}
```
#### setiosflags(ios_base::fixed) 与 setprecision(n)

setiosflags 是包含在命名空间iomanip 中的C++ 操作符，该操作符的作用是执行由有参数指定区域内的动作；iso::fixed 是操作符setiosflags 的参数之一，该参数指定的动作是以带小数点的形式表示浮点数，并且在允许的精度范围内尽可能的把数字移向小数点右侧。

setprecision 也是包含在命名空间iomanip 中的C++ 操作符，该操作符的作用是设定浮点数；setprecision(2) 的意思就是小数点输出的精度，即是小数点右面的数字的个数为2。

iso::right 也是setiosflags 的参数，该参数的指定作用是在指定区域内右对齐输出。

>cout<<setiosflags(ios::fixed)<<setiosflags(ios::right)<<setprecision(2);
合在一起的意思就是，输出一个右对齐的小数点后两位的浮点数。
