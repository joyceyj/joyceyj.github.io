---
layout: post
title: Insert Sort直接插入排序
date: 2015-11-06
categories: C_language
tag: C C++
---
# Insert Sort直接插入排序

## 思想


* 每一轮都有一个元素在最终的位置
* 优化：
* 直观解释：[插入排序](https://www.bilibili.com/video/av10076626?from=search&seid=3282567596778482261)

## 代码
```C
#include "stdio.h"
void insert_sort(int* nums, int n) {
    int i, j, tmp;
    for (i = 0; i < n; i++) {
        for (j = i+1; j >= 0; j--) {
            if (nums[j] < nums[j-1]) {
                tmp = nums[j];
                nums[j] = nums[j-1];
                nums[j-1] = tmp;
            } else {
                break;
            }
        }
    }
}
int main() {
    int nums[] = {3,2,5,1,17,4,10,13};
    int n = 8, i;
    insert_sort(nums, n);
    for (i = 0; i < n; i++) {
        printf("%d ", nums[i]);
    }
    return 0;
}
```


## 时间复杂度
||时间复杂度|趟|比较次数|
|----|----|----|----|
|最优时间复杂度|O()|1|n-1
|最坏时间复杂度|O(n^2)|n-1|n*(n-1)/2
|平均时间复杂度|O(n^2)||

## 稳定性
插入排序是一个稳定的排序方法




