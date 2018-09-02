---
layout:     post
title:      LeetCode-122.买卖股票的最佳时机 II | Best Time to Buy and Sell Stock II
subtitle:   贪心算法 列表 列表解析
header-img: img/post-bg-cyber-city.jpg
data:       2018-09-02
author:     DF
catalog:    true
tags:
    - LeetCode
    - 贪心算法
    - 列表
    - 列表解析
    - Python 
---

> [题目链接](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/description/){:target="_blank"}


## 题目

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**
```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

**示例 2:**
```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3:**
```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```
## 解答

### 分析
从题目中分析，要求只能保留**一只**股票，并且尽可能完成**多**的交易。

于是我开始尝试从从第一个低于第二天价格的位置买入，然后在第一个高于此价格的位置卖出，然
后再从下一个低于后续价格的位置买入，然后在最近的高于此价格位置卖出。经过计算，我发现按照这种方式，所有的买入和卖出操作都是发生在相邻元素的，也就是列表的 i和i+1位置。并且在计算了实例的三个用例后，结果符合预期。同时，这也符合 [贪心算法](https://zh.wikipedia.org/wiki/%E8%B4%AA%E5%BF%83%E6%B3%95) [^tanxin] 的思路。 

[^tanxin]: 贪心算法：是一种在每一步选择中都采取在当前状态下最好或最优（即最有利）的选择，从而希望导致结果是最好或最优的算法。

进而得出结论，只要计算列表中所有相邻元素的差值，最大收益即为所有差值大于0的结果的和。

从列表长度方面分析，当列表为空，或者只有一个元素的时候，不满足买入卖出条件，所以可以在计算时先行排除。

### 方式一

```python
class Solution1:
    """
    首先判断 prices长度是否小于2,
    然后循环结算列表 i+1 与 i 的差, 
    并将大于0的结果累加。
    最后返回累加结果,即为最大收益
    """

    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        max_profit = 0
        if len(prices) < 2:
            return max_profit
        for i in range(len(prices) - 1):
            profit = prices[i + 1] - prices[i]
            if profit > 0:
                max_profit += profit
        return max_profit
```

- 时间复杂度：O(N) ， 空间复杂度： O(1)

![bttbassii-2](/img_blog/bttbassii-2.jpg)


### 方式二

只是写了一个列表解析[^jiexi]，一行完成。 实际性能不如方式一。

[^jiexi]: [一篇关于python列表解析的帖子](http://codingpy.com/article/python-list-comprehensions-explained-visually/)

```python
class Solution2:
"""
解题思路与方式一相同，列表 i+1 与 i 元素相减，并将大于0的结果相加。
只是改用了列表解析的方式，来写成一行。 
"""

    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        return int((sum(list((prices[i + 1] - prices[i]) + abs(prices[i + 1] - prices[i])for i in range(len(prices) - 1)))) / 2)
```

- 时间复杂度：O(N)，空间复杂度：O(N)

![bttbassii-1](/img_blog/bttbassii-1.jpg)


## 思考
由于是刚开始接触算法题，最开始看到这道题的时候有些摸不着头脑。不知道从哪里入手，比如怎么找出差值最大的和，是先判断间隔为1的差值，在判断间隔为2的差值...
但是沉下心来，在纸上进行一些尝试后，找出适用于用例的计算方式，很快就找到了这个规律，直接找出相邻的差值即可。然后写成代码进行验证，符合预期。