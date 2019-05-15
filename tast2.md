我们可以用栈实现浏览器的前进后退功能

那么栈又是什么？

​    栈就像一叠盘子，从下一个一个往上放。**后进先出，先进后出**，就是典型栈的结构。

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_1.png)

​    

​    从操作特性上来看，栈是十分受限制的一种数据结构，只有一端能够操作，但因为只暴露了一端的操作接口，便不容易出错，更可控。**因此当数据集合满足先进后出，后进先出的特点时，就应该选择栈**。

​    

​    **顺序栈**：用数组实现的栈

​    **链式栈**：用链表实现的栈

​    

​    Java实现的顺序栈：

```
 1  // 基于数组实现的顺序栈
 2         public class ArrayStack {
 3           private String[] items;  // 数组
 4           private int count;       // 栈中元素个数
 5           private int n;           // 栈的大小
 6         
 7           // 初始化数组，申请一个大小为 n 的数组空间
 8           public ArrayStack(int n) {
 9             this.items = new String[n];
10             this.n = n;
11             this.count = 0;
12           }
13         
14           // 入栈操作
15           public boolean push(String item) {
16             // 数组空间不够了，直接返回 false，入栈失败。
17             if (count == n) return false;
18             // 将 item 放到下标为 count 的位置，并且 count 加一
19             items[count] = item;
20             ++count;
21             return true;
22           }
23           
24           // 出栈操作
25           public String pop() {
26             // 栈为空，则直接返回 null
27             if (count == 0) return null;
28             // 返回下标为 count-1 的数组元素，并且栈中元素个数 count 减一
29             String tmp = items[count-1];
30             --count;
31             return tmp;
32           }
33         
```

​        

​        

​        不管顺序栈还是链表栈，存储数据是只需要一个大小为n的数组就够了，而出栈入栈的操作，只需要一两个临时变量空间，因此**空间复杂度为O(1)**。

​        这里要注意，**空间复杂度并不是指 原本存储数据的空间，而是指算法执行时还需要额外的存储空间**。

​        出栈入栈的时间复杂度均为O(1)。

​        

​        

**支持动态扩容的顺序栈**

​    前面讲的数组若要实现动态扩容便是将数组数据复制到更大的内存去，对于动态扩容的顺序栈，我们只需要底层是一个动态扩容的数组。

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_2.png)

​    

​    对于动态扩容的数组平时开发用到不多，但我们主要对其进行复杂度分析。

​    它出栈复杂度都是O(1)。

​    对于入栈(push)，最好情况时间复杂度是O(1),最坏时间复杂度是O(n)。那么它的均摊时间复杂度是多少呢？

​    便于分析假设

- - 栈空间不够时，重新申请原来两倍大小的空间。
  - 没有出栈操作
  - 不涉及内存搬移的入栈操作为simple_push，时间复杂度为O(1)。

 

假设当前栈大小为K，若栈已满，push需要申请两倍内存，并进行K个数据搬移，再进行push,操作复杂度为O(K)。但有2K的空间后，接下来K-1次push都为simple-push，时间复杂度为O(1)。如果还有push，再次循环下去。

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_3.png)

​    

​        

​        讲需要数据搬移的push均摊下到k-1次的simple的入栈，便可知push的均摊时间复杂度为O(1)。

​        

​        

栈在函数调用的应用

​    操作系统给每个线程分配了一块独立的内存空间，这个内存会被组织成”栈”，用来存放临时变量。    

```
 1 int main() {
 2            int a = 1; 
 3            int ret = 0;
 4            int res = 0;
 5            ret = add(3, 5);
 6            res = a + ret;
 7            printf("%d", res);
 8            reuturn 0;
 9         }
10         
11         int add(int x, int y) {
12            int sum = 0;
13            sum = x + y;
14            return sum;
15         }
16         
```

​        上面代码变量会如下图入栈

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_4.png)

​        

​        

**栈在表达式中的应用**

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_5.png)

​    在第5步到第6步时，“-”的优先级小于“*”，便先进行出栈乘法运算，而“-”又小于从左到右的“+”的优先级，加法也出栈运算。

​    

​    

**栈在括号匹配中的运用**

​    例如“{[()]}”

​    从左到右，遇到左括号便入栈，当遇到“）”时，便从栈顶取出一个“（”，看是否匹配，若不匹配则为非法，若匹配，则继续向右扫描。

​    最后当扫描完所有的括号时，若栈为空，则格式合法。非空，则非法。

​    

​    

​    

**用栈实现浏览器前进后退功能**

​    使用连个栈，将首次浏览的页面存入X

​    

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_6.png)

​    

​    后退两次回到a页面

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_7.png)

​    使用前进又回到了b页面

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_8.png)

​    这时候，你浏览了d页面，则c页面会在Y被清空，无法回到c页面了

![img](/Users/mac/Desktop/Study/polar transformer networks based tracker/image_md/栈_9.png)



Valid Parentheses（有效的括号）

英文版：https://leetcode.com/problems/valid-parentheses/

中文版：https://leetcode-cn.com/problems/valid-parentheses/

```python
class Solution:
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        dic = {'(':')','{':'}','[':']'}
        answer = ['k']
        for i in s:
            if dic.get(answer[-1]) != i:
                answer.append(i)
            else:
                del answer[-1]
        if answer == ['k']: return True

```

Longest Valid Parentheses（最长有效的括号）[作为可选]

英文版：https://leetcode.com/problems/longest-valid-parentheses/

中文版：https://leetcode-cn.com/problems/longest-valid-parentheses/

      class Solution:
        def longestValidParentheses(self, s: str) -> int:
            if len(s) <= 1:
                return 0
            res = 0
            start = 0
            stc = []
            for i in range(len(s)):
                if s[i] == '(':
                    stc.append(i)
                else:
                    if (len(stc) == 0):
                        start = i + 1
                    else:
                        stc.pop()
                        if len(stc) == 0:
                            res = max(res, i - start + 1)
                        else:
                            res = max(res, i - stc[len(stc) - 1])
    
            return res
Evaluate Reverse Polish Notatio（逆波兰表达式求值）

英文版：https://leetcode.com/problems/evaluate-reverse-polish-notation/

中文版：https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/

```python
class Solution:

@param tokens, a list of string

@return an integer

    def evalRPN(self, tokens):
        stack = []
        for i in range(0,len(tokens)):
            if tokens[i] != '+' and tokens[i] != '-' and tokens[i] != '' and tokens[i] != '/':
                stack.append(int(tokens[i]))
            else:
                a = stack.pop()
                b = stack.pop()
                if tokens[i] == '+':
                    stack.append(a+b)
                if tokens[i] == '-':
                    stack.append(b-a)
                if tokens[i] == '':
                    stack.append(ab)
                if tokens[i] == '/':
                    if ab < 0:
                        stack.append(-((-b)/a))
                    else:
                        stack.append(b/a)
        return stack.pop()

```



- 【队列】

用数组实现一个顺序队列

用链表实现一个链式队列

实现一个循环队列



链表实现队列：

　　尾部 添加数据，效率为0(1)　　

　　头部 元素的删除和查看，效率也为0(1)

顺序表实现队列：

　　头部 添加数据，效率为0(n)　　

　　尾部 元素的删除和查看，效率也为0(1)

循环顺序表实现队列：

　　尾部 添加数据，效率为0(1)　　

　　头部 元素的删除和查看，效率也为0(1)



# 2. 队列

## 2.1用数组实现一个顺序队列

```
#数组实现的队列
from typing import Optional

class ArrayQueue:
    def __init__(self, capacity: int):
        self._items = []
        self._capacity = capacity
        self._head = 0
        self._tail = 0

    def enqueue(self, item: str) -> bool:
        if self._tail == self._capacity:
            if self._head == 0:
                return False
            else:
                for i in range(0, self._tail - self._head):
                    self._items[i] = self._items[i + self._head]
                self._tail = self._tail - self._head
                self._head = 0

        self._items.insert(self._tail, item)
        self._tail += 1
        return True

    def dequeue(self) -> Optional[str]:
        if self._head != self._tail:
            item = self._items[self._head]
            self._head += 1
            return item
        else:
            return None

    def __repr__(self) -> str:
        return " ".join(item for item in self._items[self._head : self._tail])
```

## 2.2用链表事项一个链式队列

## 2.3实现一个循环队列

```
from typing import Optional
from itertools import chain
#循环队列

class CircularQueue:
    def __init__(self, capacity):
        self._items = []
        self._capacity = capacity + 1
        self._head = 0
        self._tail = 0

    def enqueue(self, item: str) -> bool:
        if (self._tail + 1) % self._capacity == self._head:
            return False

        self._items.append(item)
        self._tail = (self._tail + 1) % self._capacity
        return True

    def dequeue(self) -> Optional[str]:
        if self._head != self._tail:
            item = self._items[self._head]
            self._head = (self._head + 1) % self._capacity
            return item

    def __repr__(self) -> str:
        if self._tail >= self._head:
            return " ".join(item for item in self._items[self._head : self._tail])
        else:
            return " ".join(item for item in chain(self._items[self._head:], self._items[:self._tail]))

if __name__ == "__main__":
    q = CircularQueue(5)
    for i in range(5):
        q.enqueue(str(i))
    q.dequeue()
    q.dequeue()
    q.enqueue(str(5))
    print(q)
```

 

- 练习：

Design Circular Deque（设计一个双端队列）

英文版：https://leetcode.com/problems/design-circular-deque/

中文版：https://leetcode-cn.com/problems/design-circular-deque/

deque支持从任意一端增加和删除元素。

```
from collections import deque

d = deque("abcdefg")
print d
print "length:", len(d)
print "left end:", d[0]
print "right end:", d[-1]

d.remove('b')
print d
```

由于deque是一种序列容器，因此同样支持list的一些操作，如用**getitem**()检查内容，确定长度，以及通过匹配标识从序列中间删除元素。

输出结果如下：

```
deque(['a', 'b', 'c', 'd', 'e', 'f', 'g'])
length: 7
left end: a
right end: g
deque(['a', 'c', 'd', 'e', 'f', 'g'])
```

### 添加元素

deque支持从任意一端添加元素。

- extend() 一次性从右端添加多个元素
- append() 从右端添加一个元素
- extendleft() 从左端添加多个元素，注意是逆序输入
- appendleft() 从左端添加一个元素

### 获取元素

- pop() 从右端移除元素
- popleft() 从左端移除元素
  注意，deque是线程安全的，所以可以在不同的线程中同时从两端移除元素。

### 旋转移动元素

```
from collections import deque

d = deque(xrange(10))
print d
d.rotate(2)
print d
d.rotate(-2)
print d
```

输出结果如下：

```
deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
deque([8, 9, 0, 1, 2, 3, 4, 5, 6, 7])
deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

当参数为正整数n时，rotate()将向右移动n位，并将队列右端的n个元素移到左端，当参数为负数-n是，rotate()将向左移动n位，并将队列左边的n个元素移动到右边。

### 统计队列中元素个数

使用count()方法统计队列中某个元素的个数。

```
from collections import deque

d = deque("abac")
d.count('a')    //2
```

### 反转队列

使用reverse方法反转队列：

```
from collections import deque

d = deque(range(10))
d.reverse()
```

### maxlen

deque还可以设置队列的长度，使用 deque(maxlen=N) 构造函数会新建一个固定大小的队列。当新的元素加入并且这个队列已满的时候， 最老的元素会自动被移除掉。

如果你不设置最大队列大小，那么就会得到一个无限大小队列。

```
from collections import deque

d1 = deque(maxlen=5)
d2 = deque(range(10),4)
```

### 应用

有限长度的deque可以提供类似于tail的功能：

```
from collections import deque
def tail(filename, n=10):
    'Return the last n lines of a file'
    return deque(open(filename), n)
```

另外一种用途是用来保留有限历史记录，比如，下面的代码在多行上面做简单的文本匹配， 并返回匹配所在行的最后N行：

```
from collections import deque


def search(lines, pattern, history=5):
    previous_lines = deque(maxlen=history)
    for li in lines:
        if pattern in li:
            yield li, previous_lines
        previous_lines.append(li)

# Example use on a file
if __name__ == '__main__':
    with open(r'../../cookbook/somefile.txt') as f:
        for line, prevlines in search(f, 'python', 5):
            for pline in prevlines:
                print(pline, end='')
            print(line, end='')
            print('-' * 20)
```

Sliding Window Maximum（滑动窗口最大值）

英文版：https://leetcode.com/problems/sliding-window-maximum/

中文版：https://leetcode-cn.com/problems/sliding-window-maximum/



```python
public class Solution {
    //单调双向队列(窗口内最大值)
    public ArrayList<Integer> maxSlidingWindow(int[] nums, int k) {
        if (nums == null || k < 1 || nums.length < k) return null;
        ArrayList<Integer> res = new ArrayList<>();
        LinkedList<Integer> qmax = new LinkedList<>();//保存的是下标
        for (int i = 0; i < nums.length; i++) {
            while (!qmax.isEmpty() && nums[qmax.peekLast()] <= nums[i]) {//要队尾满足条件
                qmax.pollLast();
            }
            qmax.addLast(i);
            if (i - k == qmax.peekFirst()) {
                qmax.pollFirst();//向左弹出  过期的数据s
            }
            if (i >= k - 1) {
                res.add(nums[qmax.peekFirst()]);
            }
        }
        return res;
    }
}


```

编程实现斐波那契数列求值 f(n)=f(n-1)+f(n-2)

```python
def Fbnq(n):

    if n ==0 or n == 1:

        return 1

    return Fbnq(n-1) +Fbnq(n-2)

print(Fbnq(3))

```

 编程实现求阶乘 n!

```python
def fun(n):
    if n<=1:
    return 1
    else:
return n*fun(n-1)
num=fun(int(raw_input()))
print num
```

编程实现一组数据集合的全排列

```python
def perm(elem_list, s=''):
    # 类型检查
    if type(elem_list) != type([]):
        return

    # 参数合法性检查
    if len(elem_list) == 0:
        return

    # 跳出条件
    if len(elem_list) == 1:
        # 打印当前结果
        print(s+elem_list[0])
        return

    # 依次挑选一个元素作为第1元素
    for i,e in enumerate(elem_list):
        # 递归调用自身，同时传入当前的前缀字符串
        perm(elem_list[:i] + elem_list[i+1:], s+e)


```

- 练习：
  Climbing Stairs（爬楼梯）

英文版：https://leetcode.com/problems/climbing-stairs/

中文版：https://leetcode-cn.com/problems/climbing-stairs/

```python
class Solution:
    # def __init__(self):
    #         self.dic = {1:1, 2:2}
    
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        # 这道题本质上是一个斐波那契数列,因为后一个数总是等于前两个数字之和!
        
        # 思路一:  Top down - TLE  因为前面已经算过的数字不能直接调用,总是需要重新计算.
        # if n == 1:
        #     return 1
        # if n == 2:
        #     return 2
        # if n > 2:
        #     return self.climbStairs(n-1) + self.climbStairs(n-2)

        # Top down + memorization (list) 对于思路一的改进
#         def climbStairs4(self, n):
#             if n == 1:
#                 return 1
#             dic = [-1 for i in xrange(n)]
#             dic[0], dic[1] = 1, 2
#             return self.helper(n-1, dic)
    
#         def helper(self, n, dic):
#             if dic[n] < 0:
#                 dic[n] = self.helper(n-1, dic)+self.helper(n-2, dic)
#             return dic[n]
    
        # Top down + memorization (dictionary)  对于思路一的改进,用一个字典来存储前面计算过的部分,减少运算时间
        # if n not in self.dic:
        #     self.dic[n] = self.climbStairs(n-1) + self.climbStairs(n-2)
        # return self.dic[n]
        
        
        
        
        # Bottom up, O(n) space  思路二:自底向上. 保存前面计算过的数字,用空间来换取时间
        # if n == 1:
        #     return 1
        # res = [0 for i in range(n)]
        # res[0], res[1] = 1, 2
        # for i in range(2, n):
        #     res[i] = res[i-1] + res[i-2]
        # return res[-1]
    
        # Bottom up, O(n) space  思路二:自底向上. 进一步压缩占用的存储空间
        if n == 1:
            return 1
        a, b = 1, 2
        for i in range(2, n):
            a,b = b,a+b
        return b

```

