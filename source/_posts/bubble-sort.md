---
title: 冒泡排序
author: Ackerman
date: 2022-06-29 21:07:06
categories:
- [Learning, 算法, 排序算法]
tags:
- 算法
- 排序算法
---

#### 原理

每次将一个待排序的记录按照其关键码的大小插入到一个有序序列中，直到记录全部有序。

#### 复杂度

时间复杂度 O(n²) 空间复杂度 O(1) 稳定排序算法

#### 实现步骤

略

<!-- more -->

#### 参考代码

{% tabs tab, 1%}

<!--tab 原始版-->

```go
func bubble_sort(nums []int) {
    for i := 0; i < len(nums); i++ {
        for j := 0; j < len(nums)-1-i; j++ {
            if nums[j] > nums[j+1] {
                nums[j], nums[j+1] = nums[j+1], nums[j]
            }
        }
    }
}
```

<!--endtab-->

<!--tab 优化版-->

使用变量 exchange，保存最后一次交换时的位置。当经过一轮循环， exchange 为0时，没有元素进行了交换，说明整个序列已经有序。此时就可以跳出循环，避免多余的判断

```python
func bubble_sort(nums []int) {
    n := len(nums) - 1
    exchange := n
    for exchange != 0 {
        end := exchange
        exchange = 0
        for j := 0; j < end; j++ {
            if nums[j] > nums[j+1] {
                nums[j], nums[j+1] = nums[j+1], nums[j]
                exchange = j
            }
        }
    }
}
```

<!--endtab-->

{% endtabs %}
