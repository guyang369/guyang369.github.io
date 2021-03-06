---
layout: post
title: "二分法"
subtitle: '二分法'
author: "GuYang"
header-img: "img/post-bg-2018.jpg"
tags:    
    - 面试题
    - 二分法
---

#### 已知 sqrt (2)约等于 1.414，要求不用数学库，求 sqrt (2)精确到小数点后 10 位。

#### * 考察点

1. 基础算法的灵活应用能力（二分法学过数据结构的同学都知道，但不一定往这个方向考虑；如果学过数值计算的同学，应该还要能想到牛顿迭代法并解释清楚）
2. 退出条件设计

#### * 解决办法
##### 1. 已知 sqrt(2)约等于 1.414，那么就可以在(1.4, 1.5)区间做二分
查找，如：
a) high=>1.5
b) low=>1.4
c) mid => (high+low)/2=1.45
d) 1.45*1.45>2 ? high=>1.45 : low => 1.45
e) 循环到 c)

##### 2. 退出条件
a) 前后两次的差值的绝对值<=0.0000000001, 则可退出

```
const double EPSINON = 0.0000000001;

double sqrt2( ){
    double low = 1.4, high = 1.5;
    double mid = (low + high) / 2;
    
    while (high - low > EPSINON){
        if (mid*mid > 2){
            high = mid;
        }
        else{
            low = mid;
        }
        mid = (high + low) / 2;
    }
    
    return mid;
}
```

