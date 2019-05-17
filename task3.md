- 排序】

实现归并排序、快速排序、插入排序、冒泡排序、选择排序、堆排序（选做）（完成leetcode上的返回滑动窗口中的最大值(239)，这是上一期第三天的任务进行保留（涉及队列可以对第二天进行整理复习））

编程实现 O(n) 时间复杂度内找到一组数据的第 K 大元素

**冒泡排序：**O(n^2)

```
class BubbleSort:
    def bubbleSort(self, A, n):
        for x in xrange(n):
            for y in xrange(n-x-1):
                if A[y] > A[y+1]:
                    A[y], A[y+1] = A[y+1], A[y]
        return A
```

**选择排序：**O(n^2)

想象成每次从一大堆数里面选出最小的数放在左边，重复直到这一大对数都被选完。

```
class SelectionSort:
    def selectionSort(self, A, n):
        for i in xrange(n-1):
            min_index = i
            for j in xrange(i+1, n):
                if A[min_index] > A[j]:
                    min_index = j
            if min_index != i:
                A[i], A[min_index] = A[min_index], A[i]
        return A
```

**插入排序：**O(n^2)

```
class InsertionSort:
    def insertionSort(self, A, n):
        for i in xrange(1, n):
            tmp = A[i]
            j = i - 1
            while tmp < A[j] and j >= 0:
                A[j+1] =  A[j]
                j -= 1
            A[j+1] = tmp
        return A
```

**归并排序：**O(n*log n)

　　分治法思想。把所有的数看成长度为1的有序区间：[1],[2],[3],[5],[2],[3]，再将相邻的区间合并成为最大长度为2的有序区间：[1,2],[3,5],[2,3]，再合并成为最大长度为4的有序区间：[1,2,3,5],[2,3]，再合并：[1,2,2,3,3,5]。 

```
class MergeSort:
    def mergeSort(self, A, n):
        if n <= 1:
            return A
        half = n / 2
        left = self.mergeSort(A[:half], half)
        right = self.mergeSort(A[half:], n-half)
        
        result = []
        i = j = 0
        while i < len(left) and j < len(right):
            if left[i] < right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1
        result += left[i:]
        result += right[j:]

        return result
```

**快速排序：**O(n*log n)

　　从这些数中随意选一个数，小于这个数的放在它左边，大于它的放右边；再在这左右两边的一堆数重复使用这个方法，直到排序结束。

```
class QuickSort:
    def quickSort(self, A, n=None):
        if not A:
            return []
        else:
            key = A[0]  # 固定每次选择最左边的数
            left = self.quickSort([n for n in A[1:] if n <= key])
            right = self.quickSort([n for n in A[1:] if n > key])
            return left + [key] + right
```

 **堆排序：**

　　在这里我们借用wiki的定义来说明： 通常堆是通过一维数组来实现的，在阵列起始位置为0的情况中 

　　　　(1)父节点i的左子节点在位置(2*i+1); 
　　　　(2)父节点i的右子节点在位置(2*i+2); 
　　　　(3)子节点i的父节点在位置floor((i-1)/2);

```
# -*- coding:utf-8 -*-

class HeapSort:
    def heapSort(self, A, n):
        # 创建大根堆
        for i in xrange(n/2 + 1, -1, -1): 
            self.max_heap_fix(A, i, n)
        # 堆排序
        for i in xrange(n-1, -1, -1):
            A[0], A[i] = A[i], A[0]
            self.max_heap_fix(A, 0, i)
        return A

    def max_heap_fix(self, A, i, n):
        """
        :param A: 大根堆、一维数组
        :param i: 预修复的子树根节点
        :param n: 大根堆总的元素数量
        """
        j = i * 2 + 1  # i的左子节点下标
        # 当i的左子节点存在时
        while j < n:
            # 当i的右子节点存在，且大于i的左子节点
            if j + 1 < n and A[j] < A[j+1]:
                j += 1
            # 当i的左右子节点都小于i时，修复大根堆结束
            if A[j] < A[i]:
                break
            # 当i的子节点大于i时，交换节点
            A[i], A[j] = A[j], A[i]
            i = j  # 将i移向于i交换的节点
            j = i * 2 + 1  # i的左子节点下标
```

**希尔排序：**

 　　插入排序是希尔排序的一种特殊情况，当希尔排序的初始步长为1时，即为插入排序。

```
class ShellSort:
    def shellSort(self, A, n):
        step = n / 2
        while step > 0:
            for i in xrange(step, n):
                tmp = A[i]
                while i >= step and tmp < A[i-step]:
                    A[i] = A[i-step]
                    i -= step
                A[i] = tmp
            step = step / 2
        return A
```

- 练习：

中文版：<https://leetcode-cn.com/problems/sliding-window-maximum/>

英文版：<https://leetcode.com/problems/sliding-window-maximum/>

```python
# -*- coding:utf-8 -*-
class Solution:
    def maxInWindows(self, num, size):
        # write code here
        # 存放可能是最大值的下标
        maxqueue = []
        # 存放窗口中最大值
        maxlist = []
        n = len(num)
        # 参数检验
        if n == 0 or size == 0 or size > n:
            return maxlist
        for i in range(n):
            # 判断队首下标对应的元素是否已经滑出窗口
            if len(maxqueue) > 0 and i - size >= maxqueue[0]:
                maxqueue.pop(0)
            while len(maxqueue) > 0 and num[i] > num[maxqueue[-1]]:
                maxqueue.pop()
            maxqueue.append(i)
            if i >= size - 1:
                maxlist.append(num[maxqueue[0]])
        return maxlist
```



- 【二分查找】

实现一个有序数组的二分查找算法

实现模糊二分查找算法（比如大于等于给定值的第一个元素）

二分查找算法

```
def binarysearch(ll,x):
    length = len(ll)
    height = length-1
    ini = 0
    
    while ini <= height:
        mid = (ini + height) // 2
        findx = ll[mid]
        
        if findx == x:
            return mid
        if findx < x:
            # 中间数比待找值小
            ini = mid + 1
        if findx > x:
            height = mid - 1
    
    return None
```

 练习：

Sqrt(x) （x 的平方根）

英文版：<https://leetcode.com/problems/sqrtx/>

中文版：<https://leetcode-cn.com/problems/sqrtx/>

    class Solution(object):
        def mySqrt(self, x):
            """
            :type x: int
            :rtype: int
            """
            if x == 0:
                return 0
            left = 1
            right = x / 2 + 1
       while left <= right:
            mid = left + (right - left) / 2
            sq = x / mid
            if sq > mid:
                left = mid + 1
            elif sq < mid:
                right = mid - 1
            else:
                return mid
        return right