
这里主要记录leetcode刷过的题。刷题列表主要来源于[NeetCode](https://neetcode.io/)。

## 0. 刷题表纯享

这里直接放置所有刷过的题列表。

[**Arrays & Hashing:**](#Arrays_Hashing)
- [x] [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/), Easy
- [x] [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/), Easy
- [x] [1. Two Sum](https://leetcode.com/problems/two-sum/), Easy
- [x] [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/), Medium
- [x] [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/), Medium
- [ ] [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/), Medium
- [ ] [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/), Medium
- [ ] [encode-and-decode-strings](https://leetcode.com/problems/encode-and-decode-strings/), Medium, 需要会员
- [ ] [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/), Medium

<br>

[**Two Pointers:**](#Two_Pointers)
- [x] [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/), Easy
- [x] [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/), Medium
- [x] [15. 3Sum](https://leetcode.com/problems/3sum/), Medium
- [ ] [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/), Medium
- [ ] [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/), Hard

<br>

[**Sliding Window:**](#Sliding_Window)
- [x] [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/), Easy
- [ ] [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/), Medium
- [ ] [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/), Medium
- [ ] [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/), Medium
- [ ] [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/), Hard
- [ ] [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/), Hard

<br>






## 1. 题解

这里放置所有刷过题的题解。

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

[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

直接对每个字符串排序，对该值计数，但如果涉及排序的话，时间复杂度（$\mathcal O(m n \log n)$）会高（但神奇的是leetcode上更快）；

依然使用`Counter`的思路，需要注意的是*Python里的字典不能使用字典和列表作为键，但是可以使用tuple作为键。tuple是不可更改的列表，也是有序的*。因此，我们用一个列表，每个位置分别记录对应字符串中出现各字母的次数；然后将该列表转换为tuple来做为键。时间复杂度为 $\mathcal{O}(mn)$，其中 $m$ 是列表长度，$n$ 是列表中字符串平均长度

<br>

[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

用最大堆不是最优解，复杂度 $\mathcal O(n \log n)$

利用Bucket Sort的思想，把 value-count 反过来，变成 count-value 的映射模式，把count直接按照bucket存进去，然后每个count下挂一个列表，包含对应计数次数的数字，时间空间复杂度都是 $\mathcal O(n)$

<br>

[238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

<br>

[36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

<br>

[128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)


### Two Pointers :id=Two_Pointers


[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

```python
s = '1a'
s.lower()
s = s.join(['1', 'n'])
for i in s:
    print(i.isalpha(), i.isnumeric())
```

<br>

[167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)


two sum的进阶版

利用数列的有序性，使用two pointer

时间 $\mathcal{O}(n)$，空间 $\mathcal O(1)$

<br>

[15. 3Sum](https://leetcode.com/problems/3sum/)


有了前面的基础，这道题就是two sum和two sum ii的结合。

$a+b+c=0$ 等价于 $a+b = -c$，所以先排序，然后遍历负的部分，转换为 $a+b$ 的two sum问题（同时注意不能加入重复答案、要搜到左右指针重合才停止）

排序是 $\mathcal{O}(n \log n)$，$n$ 个two sum是 $\mathcal{O}(n^2)$，最后应是 $\mathcal O(n^2)$ 

<br>

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

<br>

[42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)



<br>

### Sliding Window :id=Sliding_Window

[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

<br>

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

<br>


[424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

<br>


[567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

<br>

[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

<br>

[239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

<br>




[asd](#0-刷题表纯享)