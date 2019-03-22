---
layout: post
title: Bubble Sort冒泡排序
date: 2015-11-06
categories: C_language
tag: C C++
---
# Bubble Sort冒泡排序

## 思想
一个无序数组，从数组头部开始（从前往后，也可以从后往前），两两比较，若逆序则交换，一轮之后最大数在最终位置，再次从头开始两两比较直到上一轮已经确定位置的前一个元素结束这一轮比较，直到每个元素都在最终位置。

* 每一轮都有一个元素在最终的位置
* 优化：如果该轮比较都没有进行位置交换，说明已全部排序，提前跳出循环
* 直观解释：[舞动的排序算法 冒泡排序](https://www.bilibili.com/video/av17004846/?spm_id_from=333.788.b_7265636f5f6c697374.2)

## 代码
```C
#include "stdio.h"
int main() {
    int nums[] = {3,2,5,1,17,4,10,13};
    int i, j, n = 8, tmp;
    bool flag = false;
    // 从前往后
    for (i = 0; i < n; i++) {
        flag = false;
        for (j = 0; j < n-i-1; j++) {
            if (nums[j] > nums[j+1]) {
                tmp = nums[j];
                nums[j] = nums[j+1];
                nums[j+1] = tmp;
                flag = true;
            }
        }
        if (!flag) break;
    }
    for (i = 0; i < n; i++) {
        printf("%d ", nums[i]);
    }
    return 0;
}
```


## 时间复杂度
||时间复杂度|趟|比较次数|
|----|----|----|----|
|最优时间复杂度|O(n)|1|n-1
|最坏时间复杂度|O(n^2)|n-1|n*(n-1)/2
|平均时间复杂度|O(n^2)||

## 稳定性
冒泡排序是一个稳定的排序方法




