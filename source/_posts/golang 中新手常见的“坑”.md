---
title: golang 中新手常见的“坑”
author: Ackerman
date: 2022-01-02 00:23:43
categories:
- [Learning, Golang]
tags:
- golang
- copy
- slice
- append
top: 99
---

### copy

#### 基本认识

- copy 函数只能用于切片，不能用于 map 等其他任何类型。

- 函数签名：

  ```go
  // The copy built-in function copies elements from a source slice into a
  // destination slice. (As a special case, it also will copy bytes from a
  // string to a slice of bytes.) The source and destination may overlap. Copy
  // returns the number of elements copied, which will be the minimum of
  // len(src) and len(dst).
  func copy(dst, src []Type) int
  ```

<!-- more -->

#### 坑点

1. copy 产生了非预期结果。

   例如：

   ```go
   package main
   
   import "fmt"
   
   func main() {
   	src := []int{1, 2, 3, 4, 5}
   	dst := []int{}
   	copy(dst, src)
   
   	fmt.Println("src len:", len(src), "src:", src)
   	fmt.Println("dst len:", len(dst), "dst:", dst)
   }
   ```
   
   运行结果如下：

   ```shell
   src len: 5 src: [1 2 3 4 5]
   dst len: 0 dst: []  // 没有拷贝过来
   ```
   
   函数签名中已经说明了，**拷贝的元素个数根据 src 与 dst 长度的最小值决定**，所以目标切片 dst 需要先初始化长度。

   使用注意事项：

   ```shell
   1.如果 dst 长度小于 src 的长度，则拷贝 src 中的部分内容
   2.如果大于，则全部拷贝过来，其余的空间填充该类型的默认值
   3.如果相等，则刚好完全拷贝过来，所以，通常在 dst 初始化时即指定其为 src 的长度
   ```
   
2. 源切片 src 中元素类型为引用类型时，拷贝的是引用。

   例如：

   ```go
   package main
   
   import "fmt"
   
   func main() {
   	src := [][]int{{1, 2}, {3, 4}, {5, 6}}
   	dst := make([][]int, len(src))
   	copy(dst, src)
   	fmt.Printf("src指针:%p, src中第一个元素的指针:%p\n", src, src[0])
   	fmt.Printf("dst指针:%p, dst中第一个元素的指针:%p\n", dst, dst[0])
   }
   ```

   运行结果如下：

   ```shell
   src指针:0xc000080050, src中第一个元素的指针:0xc0000140e0
   dst指针:0xc0000800a0, dst中第一个元素的指针:0xc0000140e0  // 与 src 中第一个元素的指针相同
   ```

   所以 copy 多维数组时，需要将每个切片元素进行初始化并拷贝，例如：

   ```go
   func main() {
   	src := [][]int{{1, 2}, {3, 4}, {5, 6}}
   	dst := make([][]int, len(src))
   	for i := 0; i < len(dst); i++ {
   		dst[i] = make([]int, len(src[i]))
   		copy(dst[i], src[i])
   	}
   	fmt.Printf("src指针:%p, src中第一个元素的指针:%p\n", src, src[0])
   	fmt.Printf("dst指针:%p, dst中第一个元素的指针:%p\n", dst, dst[0])
   }
   ```

   运行结果如下：

   ```shell
   src指针:0xc000080050, src中第一个元素的指针:0xc0000140e0
   dst指针:0xc0000800a0, dst中第一个元素的指针:0xc000014110
   ```



### 对 slice 进行 append

#### 基本认识

slice 的结构

```go
type slice struct {
	Ptr *int //指向分配的数组的指针
	Len int  // 长度
	Cap int  // 容量
}
```

#### 坑点

1. 修改切片有时会影响原数组，有时不会。

   例如：

   ```go
   package main
   
   import "fmt"
   
   func main() {
       arr1 := [5]int{1, 2, 3, 4, 5}
       slice1 := arr1[:2]
       // len(slice1) = 2
       // cap(slice1) = 5
       slice1 = append(slice1, 6, 7)
       fmt.Printf("arr1:%v slice1:%v\n", arr1, slice1)
   
       arr2 := [5]int{1, 2, 3, 4, 5}
       slice2 := arr2[:3]
       // len(slice2) = 3
       // cap(slice2) = 5
       slice2 = append(slice2, 6, 7, 8)
       fmt.Printf("arr2:%v slice2:%v\n", arr2, slice2)
   }
   ```
   
   运行结果：
   
   ```shell
   arr1:[1 2 6 7 5] slice1:[1 2 6 7]  // 原数组被改变了
   arr2:[1 2 3 4 5] slice2:[1 2 3 6 7 8]  // 原数组没有改变
   ```

   从以上结果可以发现，arr1 的值被“莫名”的改变了，arr2 的值却没有改变，这貌似不符合我们的预期。

   这里的原因是由于 slice 的容量值，限定了 slice 可容纳元素的最多个数，当我们往 slice 里添加新元素，导致元素个数超过容量时（len>cap），则需要对slice进行扩容（Growing slices）。append 方法的调用就是典型的扩容示例。
   
   扩容步骤：

   1. 创建一个容量更大的 slice （扩容）。与对 slice 进行切片操作不同，这个 slice 是全新的，它的数组也是全新的，指针也是指向新数组的首位置。
   2. 新 slice 创建好后，会将原来被 append 的 slice 的元素内容进行值复制到新的 slice。
   3. 将要被 append 元素，追加到新slice的末尾。
   
   因此：
   
   1. **slice 因容量不足而进行扩容后，指向的底层数组已经变了，即便追加元素，也不会影响原数组**。
   2. slice 容量足够时，一切修改都是针对原数组的。



### Reference

- [golang浅拷贝与深拷贝](https://juejin.cn/post/7025245076867514381)
- [对Go的Slice进行Append的一个坑](http://sharecore.net/post/%E5%AF%B9go%E7%9A%84slice%E8%BF%9B%E8%A1%8Cappend%E7%9A%84%E4%B8%80%E4%B8%AA%E5%9D%91/)
