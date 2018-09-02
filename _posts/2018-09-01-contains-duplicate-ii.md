---
layout:     post
title:      LeetCode-219.存在重复元素 II | Contains Duplicate II
subtitle:   哈希表 列表 字典
header-img: img/post-flying-colors.jpg
data:       2018-09-01
author:     DF
catalog:    true
tags:
    - LeetCode
    - 哈希表
    - 列表
    - 字典
    - Python
---

>[题目链接](https://leetcode-cn.com/problems/contains-duplicate-ii/description/){:target="_blank"}

## 题目

给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，并且 i 和 j 的差的绝对值最大为 k。

示例 1:

```
输入: nums = [1,2,3,1], k = 3
输出: true
```
示例 2:

```
输入: nums = [1,0,1,1], k = 1
输出: true
```
示例 3:

```
输入: nums = [1,2,3,1,2,3], k = 2
输出: false
```

## 解决方案

### 思路

这道题，可以直接使用一个**长度为k的哈希表**即可解决问题。哈希表中只会保存不重复的内容，所以通过循环，按顺序每次向哈希表中添加一个列表元素，判断哈希表长度是否增长：如果增长，就将最早加入哈希表的元素删除，以保持哈希表长度位k；如果长度不变，即找到了列表中满足重复且索引差的绝对值最大为k的元素，题目结束。

这就相当与在一条数字组成的轨道上放置的一个长度为k的方框，只要判断框框范围内是否有重复即可，而不需要从一开始就找到所有重复元素。

**此外**，还可以在程序开头，先行判断：
1. 列表是否存在重复
2. 列表长度是否超过k

**补充**：
这道题也可以使用字典来实现类似哈希表的操作，用key存nums[i]的值,value 存 nums[i]的索引。每当发现重复元素时（if xx in dic），判断字典中对应key的 value值是与当前元素的索引值的差，如果符合!>k 这个条件，即找到结果，反之则更新字典中key的value值。

### 方法一： 

通过set来解决问题

```python
class Solution:
    """
    给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，并且 i 和 j 的差的绝对值最大为 k。
    """

    # 需要使用 haseSet来解决问题
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype bool

        使用hash set 来判断 k长度的窗口中有无重复的数字
        """
        # 判断nums为空的情况
        if not nums: return False
        # 判断nums长度小于k的情况
        if len(nums) <= k:
            return len(nums) > len(set(nums))
        # 判断读取的前k个元素有无重复
        hashset = set(nums[:k])
        if len(hashset) < k:
            return True
        # 循环判断,每次按顺序添加一个元素到set中,判断长度.如果为k,证明新加入的元素与set中的存在重复,条件成立.
        # 否则,就将set中最早加入的元素去掉, 由于set()后会改变原nums顺序,所以需要使用remove方法,按照添加顺序去除. 即nums[i-k]
        for i in range(k, len(nums)):
            hashset.add(nums[i])
            if len(hashset) == k:
                return True
            else:
                hashset.remove(nums[i - k])  # 这里相当于去掉了nums的第一个元素,依次类推
        return False
```
- 时间复杂度：O(N), 空间复杂度 O(1)

![cdii-1](/img_blog/cdii-1.jpg)
**beat 98.67 % 的 python3**

### 方法二：

通过使用字典来解决问题,用key存nums[i]的值,value寸nums[i]的索引


```python
class Solution:
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        #利用字典存储数据，以便查询，key为数字，value为对应索引
        dic = {}
        for index, value in enumerate(nums):
            if value in dic and index - dic[value] <= k:
                return True
            else:
                dic[value] = index
                # Q: 如果发现重复的数字但是不满足k的条件,如何处理
                # A: dic使用value作为key,所以只会保留一个数字最新的索引,不满足条件就会被覆盖
        return False
```
- 时间复杂度：O(N), 空间复杂度 O(N)

## 思考：

看到这道题时，首先想到的思路是分步进行操作：
1. 判断列表是否存在重复元素
2. 找出重复的元素，并返回set()后的结果
3. 找出每个重复元素在列表中的所有位置的索引
4. 判断这些索引之间的差，是否存在 不超过k的结果。
我按照这个思路分块写出了代码后，逻辑没有任何问题，但是用时超出时间限制。

经过分析和梳理，发现自己把简单问题复杂化了，上面使用的这个set(),就可以直接解决问题。

在练习算法题的时候，如果自己安装逻辑方式的解法不满足题目要求，尝试多动手在本子上画一画，或者说进行一些空间想象，可能会找出更好的方案。例如这道题，其实就像一个数组轨道上加一个长度为k的方框这样的东西。不需要考虑方框外的情况，只要确认方框内的数组满足条件即可。

最开始，想的很复杂的解决方式：
![emm](/img_blog/emmm.jpg)
