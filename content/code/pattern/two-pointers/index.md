---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Pattern-3 Two Pointers"
subtitle: ""
summary: "双指针"
authors: []
tags: ["pattern"]
categories: []
date: 2020-05-23T17:45:10+08:00
lastmod: 2020-05-23T17:45:10+08:00
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

在**有序**数组或链表中查找出满足某些限定条件一组元素时，常用双指针法。



### 2、pair with target sum

> 给定有序数组、目标值target，在数组中找到一对值，使得其和等于目标值
>
> 返回满足条件的对值的索引

```c++
input: [1,2,3,4,6] ,target=6

output:	[1,3]

2+4=6，index=[1,3]
```

```c++
input: [2,5,9,11] ,target=11

output:	[0,2]

2+9=6，index=[0,2]
```

code:

双指针法：

```c++
pair<int, int> search(const vector<int> &arr, int targetSum) {
    //指向起始，结束位置。
    int left = 0;
    int right = arr.size() - 1;
    
    while (right > left) {
        int currentSum = arr[left] + arr[right];
        if (currentSum == targetSum) {
            return make_pair(left, right);
        }
        if (targetSum > currentSum) {
            left++;
        }
        if (targetSum < currentSum) {
            right--;
        }
    }
    return make_pair(-1, -1);
}
```

Time:	*O*(N) 

Space:	*O*(1)

哈希表：

```c++
pair<int,int> searchhash(const vector<int> &arr,int targetSum){
    unordered_map<int,int> nums;
    for(int i=0;i<arr.size();i++){
        if(nums.find(targetSum-arr[i])!=nums.end()){
            return make_pair(nums[targetSum-arr[i]],i);
        }else{
            nums[arr[i]]=i;
        }
    }
    return make_pair(-1,-1);
}
```

Time:	*O*(N) 

Space:	*O*(N)

### 3、remove Duplicates

> 删除有序数组中重复的元素，不准使用额外的存储空间，返回删除后的新数组长度

```c++
input:	[2,3,3,3,6,9,9] 

output: 4

after remove:[2,3,6,9]
```

code:

```c++
int remove(vector<int> &arr) {
    //nextNonDuplicate前面全是不重复的元素，同时承担计数功能
    //nextNonDuplicate和i都是指针
    int nextNonDuplicate = 1;
    for (int i = 1; i < arr.size(); i++) {
        if (arr[nextNonDuplicate - 1] != arr[i]) {
            arr[nextNonDuplicate] = arr[i];
            nextNonDuplicate++;
        }
    }
    return nextNonDuplicate;
}
```

Time Complexity :	*O*(N) 

Space Complexity :	*O*(1)

相似问题：

> 删除有序数组中所有等于Target的元素，返回删除后的新数组长度

```c++
input:	[3, 2, 3, 6, 3, 10, 9, 3],key=3

output: 4

after remove:[2,3,10,9]
```

code:

```c++
int removeTaget(vector<int> &arr, int key) {
    // nextElement代表不等于key的元素的新位置,同时计数
    // nextElement和i都是指针
    int nextElement = 0;
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] != key) {
            arr[nextElement] = arr[i];
            nextElement++;
        }
    }
    return nextElement;
}
```

Time Complexity :	*O*(N) 

Space Complexity :	*O*(1)

### 4、squaring a sorted arrary

> 给定有序数组，将所有元素的平方有序输出

```c++
input:	[-2, -1, 0, 2, 3]

output: [0 ,1 ,4 ,4 ,9]
```

```c++
input:	[-3, -1, 0, 1, 2]

output: [0 ,1 ,4 ,4 ,9]
```

code:

```c++
vector<int> makeSqueare(vector<int> arr) {
    int n = arr.size();
    vector<int> square(n);
    int highestSquareIndex = n - 1;
    //双指针
    int right = n - 1;
    int left = 0;
    
    while (right >= left) {
        int leftSquare = arr[left] * arr[left];
        int rightSquare = arr[right] * arr[right];
        if (leftSquare > rightSquare) {
            square[highestSquareIndex] = leftSquare;
            highestSquareIndex--;
            left++;
        } else {
            square[highestSquareIndex] = rightSquare;
            highestSquareIndex--;
            right--;
        }
    }
    return square;
}
```

Time Complexity :	*O*(N) 

Space Complexity :	*O*(N)





### 5、triplet sum to zero

> 给定未排序的整数数组，找出所有和为0、长度为3且不重复的子数组

> X+Y+Z=0  -> X+Y=_Z

> 为了确保不重复，排序后的数组，重复元素相邻可略过

```c++
input:	[-3, 0, 1, 2, -1, 1, -2]

output: [[-3 1 2 ][-2 0 2 ][-1 0 1 ]]
```

```c++
input:	[-5, 2, -1, -2, 3]

output: [[-5 2 3 ][-2 -1 3 ]]
```

code:

```c++
void searchPair(const vector<int> &arr, int targetSum, int left, vector<vector<int>> &triplets) {
    //双指针
    int right = arr.size() - 1;
    while (left < right) {
        int currentSum = arr[left] + arr[right];
        if (currentSum == targetSum) {
            triplets.push_back({-targetSum, arr[left], arr[right]});
            left++;
            right--;

            //略过重复元素
            while (left < right && arr[left] == arr[left - 1]) {
                left++;
            }
            //略过重复元素
            while (left < right && arr[right] == arr[right]) {
                right--;
            }
        } else if (targetSum > currentSum) {
            left++;
        } else {
            right--;
        }
    }
}

vector<vector<int>> searchTriplets(vector<int> &arr) {
    sort(arr.begin(), arr.end());
    vector<vector<int>> triplets;
    for (int i = 0; i < arr.size() - 2; i++) {
        //略过重复元素
        if (i > 0 && arr[i] == arr[i - 1]) {
            continue;
        }
        searchPair(arr, -arr[i], i + 1, triplets);
    }
    return triplets;
}
```

Time Complexity :	*O*(N^2^) 

Space Complexity :	*O*(N)

### 6、triplet sum close to target

> 给定未排序数组和target值，找出长度为3的子数组，使得子数组的和尽可能接近target，返回子数组的和

```c++
input:	[-2, 0, 1, 2],target=2

output: 1

triplet: [-2,1,2]
```

```c++
input:	[-3, 1, 1, 2],target=1

output: 0

triplet: [-3,1,2]
```

code:

```c++
int searchTriplet(vector<int> &arr, int targetSum) {
    sort(arr.begin(), arr.end());
    int smallestDifference = INT_MAX;
    for (int i = 0; i < arr.size() - 2; i++) {
        int left = i + 1;
        int right = arr.size() - 1;
        while (left < right) {
            int targetDiff = targetSum - arr[i] - arr[left] - arr[right];
            if (targetDiff == 0) {
                return targetSum - targetDiff;
            }
            if (abs(targetDiff) < abs(smallestDifference)) {
                smallestDifference = targetDiff;
            }
            if (targetDiff > 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    return targetSum - smallestDifference;
}
```

Time Complexity :	*O*(N^2^) 

Space Complexity :	*O*(N)

### 7、triplets with smaller sum

> 给定未排序数组和target值，找出所有triplets，使得其和小于target，返回满足条件的triplets的个数

```c++

```

