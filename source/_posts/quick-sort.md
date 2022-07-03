---
title: 快速排序
author: Ackerman
date: 2022-07-03 11:30:47
categories:
- [Learning, 算法, 排序算法]
tags:
- 算法
- 排序算法
---

#### 原理

快排又称为分区交换排序，其基本思想是：首先选取一个基准值 pivot，每次都将序列划分为两部分，左侧记录的关键码小于基准值，右侧记录的关键码均大于基准值，然后对左右两部分分别重复上述划分操作，直至序列整体有序

#### 复杂度

时间复杂度 O(nlogn) 空间复杂度 O(1) 不稳定排序

#### 实现步骤

<!-- more -->

#### 参考代码

```go
func quickSort(arr []int, low, high int) []int {
    if low < high {
        arr, pivot := partition(arr, low, high)
        arr = quickSort(arr, low, pivot-1)
        arr = quickSort(arr, pivot+1, high)
    }
    return arr
}

func partition(arr []int, low, high int) ([]int, int) {
    i := low
    pivot := arr[high]
    for j := low; j < len(arr); j++ {
        if arr[j] < pivot {
            arr[i], arr[j] = arr[j], arr[i]
            i++
        }
    }
    arr[i], arr[high] = arr[high], arr[i]
    return arr, i
}

func quickSortStart(arr []int) []int {
    return quickSort(arr, 0, len(arr)-1)
}
```

#### Reference

1. https://blog.boot.dev/golang/quick-sort-golang/
