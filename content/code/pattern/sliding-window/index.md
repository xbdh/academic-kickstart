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

适用于求**连续**子数组

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
>
> 其中数组元素为正数，K为正数

```c++
input:	[2,1,5,1,3,2], k=3

output:	9
    
subarray:[5,1,3]
```

```c++
input:	[2,3,4,1,5] ,k=2

output:	7
    
 subarray:[3,4]
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



### 3、smallest subarray whose sum is greater than or equal to S

> 求最短长度的子数组，使得其和大于等于 S
>
> 其中数组元素为正数，S 为正数；如果子数组不存在，返回0

```c++
input:	[2,1,5,2,3,2], S=7
    
output:	2

subarray:[5,2]
```

```c++
input:	[3,4,1,1,6],S=8
    
output:	3
    
subarray:[3,4,1],[1,1,6]
```

code：

```c++
int findMinSubarray(int k, vector<int> &arr){
    int windowSum = 0;
    int minLength = INT_MAX;
    int windowStart = 0;
    
    for (int windowEnd = 0; windowEnd < arr.size(); windowEnd++){
        windowSum += arr[windowEnd];
        
        //当和大于等于K时候，减小窗口大小
        //不确定减去窗口第一个值后，是否满足要求，所以不断减，直到满足K
        while (windowSum >= k){
            //记录满足条件的最小窗口长度
            minLength = min(minLength, windowEnd - windowStart + 1);
            windowSum -= arr[windowStart];
            windowStart++;
        }
    }
    return minLength == INT_MAX ? 0 : minLength;
}
```

Time:	*O*(N) 

Space:	*O*(1)

### 4、longest subarrary with no more than K distinct characters

> 求包含 不超过K个不同字符 的最长子串的长度

```c++
input:	string="araaci" ,k=2

output：	4
    
substring:"araa"
```

```c++
input:	string="cbbebi" ,k=3

output：	5
    
substring:"cbbeb" "bbebi"
```

code:

```c++
int findLength(int k, const string &str) {
    int maxLength = 0;
    int windowStart = 0;
    
    //记录处理过的字符的频率
    unordered_map<char, int> mp;
    
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
        char rightchar = str[windowEnd];
        mp[rightchar]++;
        //不断减小窗口长度，直到满足K个不同字符
        while (mp.size() > k) {
            char leftchar = str[windowStart];
            mp[leftchar]--;
            //删除值为0的键
            if (mp[leftchar] == 0) {
                mp.erase(leftchar);
            }
            windowStart++;
        }
        //记录满足K的最大长度
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;
}

```

Time:	*O*(N) 

Space:	*O*(K)



### 5、maximum number of fruits in each basket

> 字符数组，每个字符代表一种果树，给2个篮子，每个篮子只能装一种水果
>
> 可以从数组任意位置装，不能回头，遇到第三种水果结束
>
> 求两个篮子能装水果的最大值

> 同4 ，K=2

code:

```c++
int findLength(int k, const string &str) {
    int maxLength = 0;
    int windowStart = 0;
    
    //记录处理过的字符的频率
    unordered_map<char, int> mp;
    
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
        mp[str[windowEnd]]++;
        //不断减小窗口长度，直到满足K个不同字符
        while (mp.size() > k) {
            mp[str[windowStart]]--;
            //删除值为0的键
            if (mp[str[windowStart]] == 0) {
                mp.erase(str[windowStart]);
            }
            windowStart++;
        }
        //记录满足K的最大长度
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;
}

```



### 6、no repeat substring

> 求无重复字符的子串的最大长度

```c++
input:	string="aabccbb"

output:	3

substring:"abc"
```

```c++
input:	string="abccde"

output:	3

substring:"abc" "cde"
```

code:

```c++
int findlength(const string &str) {
    int windowStart = 0;
    int maxLength = 0;
    unordered_map<char, int> mp;

    for (int windowEnd = 0; windowEnd < str.size(); windowEnd++) {
        char rightChar = str[windowEnd];
        if (mp.find(rightChar) != mp.end()) {
            windowStart = max(windowStart, mp[rightChar] + 1);

        }
        mp[rightChar] = windowEnd;
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;

}
```

Time:	*O*(N) 

Space:	*O*(K)

