---
layout: post
title: LeetCode 424 Longest Repeating Character Replacement
date: 2019-07-13T13:43:27.794Z
author: Delusion
tags:
  - cpp
  - LeetCode
image: /asset/2018-04-21.jpg
---

# LeetCode 424 Longest RepeatingCharacterReplacement

## 题目

> Given a string s that consists of only uppercase English letters, you can perform at 
> most k operations on that string.In one operation, you can choose any character of the 
> string and change it to any other uppercase English character.Find the length of the longest
> sub-string containing all repeating letters you can get after performing the above operations.

## 题意

本题的意思是，给定一个只含有小写字母的单词，最多可以进行K次替换，找到长度最长的相同字母字串。

本题一开始看的时候还以为是dp = =，后来想起来我最近刷的全是Two Pointers类型… 所以就往Slide Window方向
想，还是基本操作，一个指向beg，一个指向end…但是后来看了Discuss，这个解法真的惊到我了，属实优雅。

用一个mp来记录slide window中经过的所有字母，但是这个里面只有两种字母，一个是数量最多的相同字母，我们用same
表示，还有0-k个字母是替换成same字母的，如果此时刚好有k个字母被替换，那么替换字母量达到最大，此时应该有
k + same == end - beg.一旦end - beg 大于 k + same,说明我们的slide window需要缩小，即beg++，这就是全部过程。

## Code

```cpp
int characterReplacement(string s, int k)
{
    int beg = 0, end = 0;
    int len = s.length();
    int same = 0;
    int mp[130];

    memset(mp, 0, sizeof(mp));

    while (end < len) {
        same = max(same, ++mp[s[end++]]);
        if (end - beg - same > k)
            --mp[s[beg++]];
    }
    return end - beg;
}

```
