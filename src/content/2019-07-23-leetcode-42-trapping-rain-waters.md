---
layout: post
title: LeetCode 42 Trapping Rain Waters
date: 2019-07-23T04:32:08.893Z
author: Delusion
tags:
  - LeetCode
  - cpp
image: /asset/2018-08-14.jpg
---

# LeetCode 42 Trapping Rain Waters

## 题目


> Given n non-negative integers representing an elevation map where the width of each bar is 1,
> compute how much water it is able to trap after raining.

![test](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

## 题解

这个题其实很有意思，一开始刷的时候，感觉无从下手，但是第二次刷就好很多，起码有个大概思路，用了
两种思路来解这道题。

- Two Pointers
- MonoStack

### Two Pointers

我们可以先理清楚最基本的问题，能Trap住雨水的，一定是有凹槽存在，而这个凹槽能容纳多少水，一定是
以它两端中最低的这端作为Base，除此之外，应该是两端各取最大值，以达到容纳最多水的效果。举个🌰。

*Lmax L R Rmax*

此时，如果LMax < Rmax,  我们可以计算L点的水量，因为此时，我们以LMax为短板，那么一定有Rmax > LMax
所以我们可以将L处的水量，即Lmax - L 加入到水量中，反之，则加入RMax - R。

```cpp
int trap(vector<int>& height)
{
    int left = 0;
    int right = height.size()-1;
    int leftMax = 0;
    int rightMax = 0;
    int res = 0;

    while (left < right)
    {
        leftMax = max(height[left], leftMax);
        rightMax = max(height[right], rightMax);

        if (leftMax > rightMax) {
            res += rightMax - height[right];
            right--;
        } else {
            res += leftMax - height[left];
            left++;
        }
    }

    return res;
}
```
### MonoStack

这个解法其实也很有意思，单调栈，顾名思义，就是栈内的元素都保持单调递增或递减，因为我们的目的是
计算容纳的水，那么就要制造高低差。我们使栈内的元素单调递减，考虑一种极限情况，1-4都为单调递减，
第5个值 > 第一个值，此时，因为第五个值大于栈顶元素，所以我们将栈顶弹出，并将栈作为一个坑，此时，
坑的高度应该为当前栈（即弹出栈顶后的栈）的栈顶与第五个值的min（显然为栈顶）与坑的高度做差，得到
此时坑的高度，坑的宽度应该为5 - 3 - 1，也就是第五个元素的index与栈顶的index相减，因宽度为1，故
再次减一，相乘即为结果。

```cpp
int trap1(vector<int>& height)
{
    stack<int> stk;
    int res = 0;
    int i = 0;
    int sz = height.size();

    while (i < sz)
    {
        if (stk.empty() || height[stk.top()] >= height[i])
            stk.push(i++);
        else {
            int cur  = height[stk.top()];
            stk.pop();
            int len = i - stk.top() - 1;
            int h = min(stk.top(), height[i]);
            res += (h - cur) * len;
        }
    }

    return res;
}
```

## 总结 
其实Two Pointer与MonoStack有相似指出，只不过Two Pointers计算的是纵向的水量，MonoStack计算的是
横向的水量。反正这个题，应该算是吃透了8. 快乐。
