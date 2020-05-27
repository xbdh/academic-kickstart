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

