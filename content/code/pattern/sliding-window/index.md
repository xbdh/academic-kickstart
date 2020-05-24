---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Pattern-2 Sliding Window"
subtitle: ""
summary: "滑动窗口"
authors: []
tags: ["pattern"]
categories: []
date: 2020-05-23T17:44:03+08:00
lastmod: 2020-05-23T17:44:03+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### 1、介绍

### 

### 2、average of subarrarys of size K 

> 求长度为K的连续子数组的平均值

```c++
input	arrary:[1,3,2,6,-1,4,1,8,2] ,K=5
    
output	[2.2,2.8,2.4,3.6,2,8]
```

code:

```c++
#include <iostream>
#include <vector>
using namespace std;

vector<double> averageOfSubarrayOfSizeK(int k,vector<int> &arr){
    vector<double> result(arr.size() -k +1);
    int windowStart =0;
    double windowSum =0;
    for(int windowEnd =0 ; windowEnd<arr.size() ;windowEnd++){
        windowSum += arr[windowEnd];
        if(windowEnd >=k-1){
            result[windowStart]=windowSum/k;
            windowSum -=arr[windowStart];
            windowStart++;
        }
    }
    return result;
}
int main() {
    vector<int> arr={1,3,2,6,-1,4,1,8,2};
    for(double d :averageOfSubarrayOfSizeK(5,arr)){
        cout<<d<<" ";
    }
    return 0;
}
```

