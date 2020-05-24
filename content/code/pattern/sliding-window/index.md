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
input:	arrary:[1,3,2,6,-1,4,1,8,2] ,K=5
    
output:	[2.2,2.8,2.4,3.6,2,8]
```

code:

```c++
vector<double> averageOfSubarrayOfSizeK(int k,vector<int> &arr){
    vector<double> result(arr.size() -k +1);
    int windowStart =0;
    double windowSum =0;
    
    for(int windowEnd =0 ; windowEnd<arr.size() ;windowEnd++){
        //加上下一个元素
        windowSum += arr[windowEnd];
        
        //到达sizeK，滑动窗口
        if(windowEnd >=k-1){
            result[windowStart]=windowSum/k; //计算平均值
            windowSum -=arr[windowStart];  //减去第一个窗口值
            windowStart++; //滑动窗口
        }
    }
    return result;
}
```



### 2、maximum sum of subarray of size K

> 求长度为K的连续子数组的最大值

```c++
input:	[2,1,5,1,3,2], k=3

output:	9
    
subarray: [5,1,3]
```

```c++
input:	[2,3,4,1,5] ,k=2

output:	7
    
 subarray:	[3,4]
```

code:

```c++
int maxSumSubarrayOfSizeK(int k ,vector<int> &arr){
    int windowStart =0;
    int windowSum=0;
    int maxSum=0;
    
    for(int windowEnd =0; windowEnd<arr.size() ;windowEnd++){
        windowSum +=arr[windowEnd];
        if (windowEnd>=k-1){
            maxSum=max(maxSum,windowSum);
            windowSum -= arr[windowStart];
            windowStart++;
        }
    }
    return maxSum;
}
```

Time:	*O*(N) 

Space:	*O*(1)