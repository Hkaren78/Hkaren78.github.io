---
layout:     post
title:      "蒙特卡罗算法（Monte Carlo method）"
subtitle:   ""
date:       2019-03-17
author:     "HK"
header-img: "img/math.jpg"
catalog: true
tags:
    - 算法
---

> 数学建模算法学习第三天(今天有点忙就先学个简单的吧)。

## 蒙特卡罗算法的定义

蒙特卡罗方法是一种**随机模拟方法**，利用大量随机输入，产生各种输出，结构概率的分布就是真实分布的近似解。又称统计模拟法、随机抽样技术。

蒙特卡罗方法是使用**随机数（或伪随机数）**来解决很多计算问题的方法。与它对应的是确定性算法。蒙特卡罗方法在金融工程学，宏观经济学，计算物理学（如粒子输运计算、量子热力学计算、空气动力学计算）等领域应用广泛。

蒙特卡洛方法的理论基础是**大数定律**。大数定律是描述相当多次数重复试验的结果的定律，根据这个定律道样本数量越多，其平均就越趋近于真实值。

## 算法应用实例

### 蒲丰氏问题

为求得圆周率值，将成为2l的一根针任意投到地面上，用针与一组相间距离为2a(l<\a)的平行线相交的频率代替概率P,再利用准确的关系式：
P=2L/(πa),得π=2l/(aP)≈2lN/(an)（N为投记次数，n为针与平行线相交次数）。

### 求圆周率

一个带有1/4圆在内的正方形单元、落在圈内（红点）的点和正方形上的所有点的比率为：pi/4，由此估算π。

![等待网络加载图片···](https://github.com/Hkaren78/Hkaren78.github.io/raw/master/img/in-post/MonteCarlo/circlepi.png)

```c++
#include <bits/stdc++.h>

#define MAX_ITERS 1000000

using namespace std;

double Rand(double L, double R)
{
    return L + (R - L) * rand() * 1.0 / RAND_MAX;
}

double GetPi()
{
    srand(time(NULL));
    int cnt = 0;
    for(int i = 0; i < MAX_ITERS; i++)
    {
        double x = Rand(-1, 1);
        double y = Rand(-1, 1);
        if(x * x + y * y <= 1)
            cnt++;
    }
    return cnt * 4.0 / MAX_ITERS;
}

int main()
{
    for(int i = 0; i < 10; i++)
        cout << GetPi() << endl;
    return 0;
}
```
### 求定积分

> [蒙特卡罗求定积分](https://cosx.org/2010/03/monte-carlo-method-to-compute-integration)

> 注意：使用蒙特卡罗方法解任何具体问题时，所使用的随机数额个数总是有限的，只要所用随机数的个数不超过伪随机数序列出现循环现象时的长度即可。

> [参考博文](http://www.cnblogs.com/ECJTUACM-873284962/p/6892022.html)