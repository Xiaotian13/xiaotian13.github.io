
这里主要记录leetcode刷过的题。刷题列表主要来源于[NeetCode](https://neetcode.io/)。

## 0. 刷题表纯享

这里直接放置所有刷过的题列表。

[**Arrays & Hashing:**](#Arrays_Hashing)

- [x] [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/), Easy
- [x] [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/), Easy
- [x] [1. Two Sum](https://leetcode.com/problems/two-sum/), Easy
- [x] [628. Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers/), Easy
- [x] [884. Uncommon Words from Two Sentences](https://leetcode.com/problems/uncommon-words-from-two-sentences/), Easy
- [x] [697. Degree of an Array](https://leetcode.com/problems/degree-of-an-array/), Easy
- [x] [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/), Medium
- [x] [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/), Medium
- [x] [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/), Medium
- [x] [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/), Medium
- [ ] [encode-and-decode-strings](https://leetcode.com/problems/encode-and-decode-strings/), Medium, 需要会员
- [x] [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/), Medium
- [ ] [31. Next Permutation](https://leetcode.com/problems/next-permutation/), Medium
- [x] [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/), Medium

<br>

[**Two Pointers:**](#Two_Pointers)
- [x] [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/), Easy
- [x] [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/), Medium
- [x] [15. 3Sum](https://leetcode.com/problems/3sum/), Medium
- [x] [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/), Medium
- [x] [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/), Hard

<br>

[**Sliding Window:**](#Sliding_Window)
- [x] [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/), Easy
- [x] [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/), Medium
- [x] [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/), Medium
- [x] [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/), Medium
- [ ] [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/), Hard
- [ ] [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/), Hard

<br>

[**Stack:**](#Stack)

- [x] [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/), Easy
- [x] [155. Min Stack](https://leetcode.com/problems/min-stack/), Medium
- [ ] [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/), Medium
- [ ] [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/), Medium
- [ ] [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/), Medium
- [ ] [853. Car Fleet](https://leetcode.com/problems/car-fleet/), Medium
- [ ] [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/), Hard

<br>



<br>

[**Math & Geometry:**](#Math_Geometry)

- [x] [66. Plus One](https://leetcode.com/problems/plus-one/), Easy

- [x] [202. Happy Number](https://leetcode.com/problems/happy-number/), Easy
- [x] [48. Rotate Image](https://leetcode.com/problems/rotate-image/), Medium
- [ ] [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/), Medium
- [x] [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/), Medium
- [x] [50. Pow(x, n)](https://leetcode.com/problems/powx-n/), Medium
- [ ] [43. Multiply Strings](https://leetcode.com/problems/multiply-strings/), Medium
- [ ] [2013. Detect Squares](https://leetcode.com/problems/detect-squares/), Medium
- [x] [593. Valid Square](https://leetcode.com/problems/valid-square/), Medium




## 1. 题解

这里放置所有刷过题的题解。

[这里](https://github.com/kobe24o/LeetCode/blob/master/LeetCode%E8%A7%A3%E9%A2%98%E6%B1%87%E6%80%BB%E7%9B%AE%E5%BD%95.md)提供了LeetCode上许多题目的中文题解。

### Arrays & Hashing :id=Arrays_Hashing

[217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

用`set()`，对比前后两个的长度；

直接用`collections.Counter`；

用哈希表

<br>

[242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)

判断两个字符串是否只是排列顺序不同

最直接，使用`Counter`计数比较，时间空间复杂度都是 $\mathcal{O}(s+t)$；

由于两个字符串的本质区别是排列组合的顺序不同，因此，如果我们都对他们进行排序，两个字符串应该是相同的。这样某些排序算法可以把空间复杂度降低到 $\mathcal{O}(1)$，但是时间复杂度上升到 $\mathcal{O}(n \log n)$

<br>

[1. Two Sum](https://leetcode.com/problems/two-sum/)

1. 暴力循环/蛮力算法（brutal force），是$C_n^2, \mathcal O(n^2)$ 

2. 先排序，然后用two-pointers，最左+最右 如果小于target，就左边指针右移，如果大于target，就右边指针左移

   排序是 $n \log n$，指针移动是 $n$，所以最后是 $\mathcal O(n\log n)$ 

   ```python
   ls = [2, 6, 4, 7]
   ls.sort() #直接改变列表；指定参数reverse=True，则排序为降序，默认是升序
   
   for i in enumerate(ls):
       print(i)
   #(0, 2)
   #(1, 6)
   #(2, 4)
   #(3, 7)
   ```

   

3. hash table $\mathcal O(n)$：所有的插入和查询操作都是 $\mathcal O(1)$

   遍历元素，如果它的补元素不在字典里，则加进去；如果补元素在字典里，则该补元素的位置和它的位置即为输出

   ```python
   # nums = [2, 4, 6, 7]
   # target = 9
   
   d = {}
           
   for i, num in enumerate(nums):
       if num not in d:
           d[target - num] = i
       else:
           return [d[num], i]
   ```

<br>

[628. Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers/)

先排序，那么乘积最大的就是最大的三个正数，或者最大的正数和最小的两个负数。应是 $\mathcal O(n \log n)$ 

但实际上，我们就是要找最大的三个正数和最小的两个负数，这样一个操作是可以通过一次遍历就得到的。通过一次遍历，找出最大的三个数，和最小的两个数。也就是 $\mathcal O(n)$ 

<br>

[884. Uncommon Words from Two Sentences](https://leetcode.com/problems/uncommon-words-from-two-sentences/)

两个字符串拼一起，找出来只出现过一次的单词；

可以直接用`collections.Counter`

```python
from collections import Counter

s = "This is an example an is"
count = Counter(s.split(' ')) # Counter({'is': 2, 'an': 2, 'This': 1, 'example': 1})
ls = [word for word, c in count.items() if c == 1] # ['This', 'example']
```

字典常用操作：`d.get(key)`， `for key, value in d.items()`
如果字典中不存在`key`，可以通过类似`d.get(key, 0)`的方式让它返回`0`

<br>

[697. Degree of an Array](https://leetcode.com/problems/degree-of-an-array/)

```python
from collections import defaultdict

c = defaultdict(int)
c[1] += 1 # defaultdict(int)对任意键的初始值设为0
```

<br>

[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

直接对每个字符串排序，对该值计数，但如果涉及排序的话，时间复杂度（$\mathcal O(m n \log n)$）会高（但神奇的是leetcode上更快）；

依然使用`Counter`的思路，需要注意的是*Python里的字典不能使用字典和列表作为键，但是可以使用tuple作为键。tuple是不可更改的列表，也是有序的*。因此，我们用一个列表，每个位置分别记录对应字符串中出现各字母的次数；然后将该列表转换为tuple来做为键。时间复杂度为 $\mathcal{O}(mn)$，其中 $m$ 是列表长度，$n$ 是列表中字符串平均长度

<br>

[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

用最大堆不是最优解，复杂度 $\mathcal O(n \log n)$

利用Bucket Sort的思想，把 value-count 反过来，变成 count-value 的映射模式，把count直接按照bucket存进去，然后每个count下挂一个列表，包含对应计数次数的数字，时间空间复杂度都是 $\mathcal O(n)$

<br>

[238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

因为不让用除法，因此可以记录从前到后连乘，从后到前连乘，即 prefix postfix 序列，这样时间和空间复杂度都是 $\mathcal O (n)$；

其实就是prefix连乘序列和postfix连乘序列错位相乘，因此这个过程可以只用一个序列完成，即不需要extra space；但不是最快的

![](fig/IMG_424EFF231707-1.jpeg ':size=40%')

<br>

[36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

暴力检查，把每个格子扫一遍，分别check三个条件
```python
'1'.isdecimal()

from itertools import product
for i in product([1, 2], [1, 2]):
    print(i)
```

<br>

[128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

如果排序的话，就很好做，但是排序是 $\mathcal O(n \log n)$；

利用`set`的查询为 $\mathcal O (1)$

<br>

[31. Next Permutation](https://leetcode.com/problems/next-permutation/)

这个是真题

<br>

[560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

构建序列 $S_i = a_1 + \cdots+a_i$，问题转化为找出所有的 $(i, j)$ 使得 $S_j - S_i = k$，用two-sum思路解子问题
注意，这两个过程可以同时进行

对于数组 $[1, 2, 3, 4]$，它的prefix sum（前缀和）数组为 $[1, 3, 6, 10]$。


### Two Pointers :id=Two_Pointers


[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

全转小写，然后直接判断；但是用了extra memory
```python
s = '1a'
s.lower()
s = s.join(['1', 'n'])
for i in s:
    print(i.isalpha(), i.isnumeric(), i.isalnum())
```

用two pointers，没必要将整个序列反过来
```python
def AlphaNum(s):
    return (ord('A') <= ord(s) <= ord('Z') or
            ord('a') <= ord(s) <= ord('z') or
            ord('0') <= ord(s) <= ord('9'))
```

<br>

[167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)


two sum的进阶版

利用数列的有序性，使用two pointers

时间 $\mathcal{O}(n)$，空间 $\mathcal O(1)$

<br>

[15. 3Sum](https://leetcode.com/problems/3sum/)


有了前面的基础，这道题就是two sum和two sum ii的结合。

$a+b+c=0$ 等价于 $a+b = -c$，所以先排序，然后遍历负的部分，转换为 $a+b$ 的two sum问题（同时注意不能加入重复答案、要搜到左右指针重合才停止）

排序是 $\mathcal{O}(n \log n)$，$n$ 个two sum是 $\mathcal{O}(n^2)$，最后应是 $\mathcal O(n^2)$ 

<br>

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

Brute force $\mathcal O(n^2)$

two pointers结合贪心，每次向里移动更小高度的指针，$\mathcal O(n)$

<br>

[42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

给定箱体，则很容易计算出箱体内能放多少水，难点在如何确定箱体
左向右滑动得到 $\max L$，右向左滑动得到 $\max R$，计算 $\min(\max L, \max R)$，这样TC和SC均为 $\mathcal O(n)$

用two pointers，因为每个bar能存储的水量由该点左侧最高值和右侧最高值这两个值中的最小值决定，并且 $\max L, \max R$ 是（不严格）单调增，因此我们每次只滑动当前最大值中较小的那个，可以确保滑动到的bar的“高”就是这个最小值。这样SC降为 $\mathcal O(1)$

### Sliding Window :id=Sliding_Window

[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

遍历一遍，记录每一步当前价格最小值，动态记录对当前最低价格而言的最高盈利。应是 $\mathcal O(n)$ 
```python
float('-inf') #最小值
```

<br>

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

用sliding window，需要用到判断新元素是否在之前的子列里，因此可以考虑用`set`，因为查询在不在里面只需要constant time
同时结合two pointers (同向)，滑动一遍只需要 $\mathcal O(n)$

<br>

[424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

从左到右扫+two pointers，如果当前子列中，长度$-$最多重复次数$\leq k$，则当前子列即可以被完全替换；如果不满足，就左移指针，直到满足
循环直到右指针到头

<br>


[567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

sliding window，每次对比滑动得到的字符串和原始字符串
注意每次滑动只变化两个元素，即窗口开始元素和窗口结束元素
处理可以类似前面Anagram
```python
ord('a')
chr()
```

<br>

[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

<br>

[239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

<br>

### Stack :id=Stack

[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

栈的基础运用

```python
stack = [1, 2, 3]
stack.pop()
```

<br>

[155. Min Stack](https://leetcode.com/problems/min-stack/)

另建一个列表，记录每加一个新元素进去的时候当前最小是多少

<br>

[150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)



<br>

[22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

<br>

[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

<br>

[853. Car Fleet](https://leetcode.com/problems/car-fleet/)

<br>

[84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)


<br>

[返回开头](#0-刷题表纯享)