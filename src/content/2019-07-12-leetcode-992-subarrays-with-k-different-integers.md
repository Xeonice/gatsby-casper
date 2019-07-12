---
layout: post
title: Leetcode 992 Subarrays With K Different Integers
date: 2019-07-10T05:40:26.142Z
author: Delusion
tags:
  - React
image: /asset/2018-08-15.jpg
---




\# LeetCode 992 SubarraysOfKDistincts 



\## 题目



Given an array A of positive integers, call a (contiguous, not necessarily distinct) subarray of  A good if the number of different integers in that subarray is exactly K. (For example, \[1,2,3,1,2] has 3 different integers: 1, 2, and 3.)Return the number of good subarrays of A.



本题的意思是说，在给定的数组中，有多少个子数组刚好含有K个数字。



\## 题解



一开始想到的是用一个Slide Window，但是只是简单的用一个deq来做滑动窗口，有可能会错过一些情况。

后来和室友讨论的方法是维护一个数组，以数组中的每个元素为起始元素来向后遍历，一直到最右端，

即当前字数组中包含的数字数量为K位置，后面设置一个特殊标志为如INF等。但是这个样子其实复杂度

较高，不具有可行性。



后在Youtube上刷到了题解，这个题完全可以以高中时候的思路来做，原题为求刚好含有K个数字，我们改为，

完整遍历一遍A数组，每次 += f(K) = 以当前字符结尾的子串至多含有K个数字。同时，使用f(k) - f(k-1)

就可以得出题目的解。 



还有一个要点是：我们计算至多含有K个数字其实有很多种解法，但是可以在遍历的时候，通过滑动窗口的两侧

的差值+1，就是以当前字符结尾的字串数量.



\## Code



\`\``cpp



int subarraysWithKDistinct(vector<int>& A, int k)

{

	int n = A.size();

	int res;



	auto sub = \[&A](int k) {

		int ans = 0;

		int i = 0;

		int mp\[20005];

		memset(mp, 0, sizeof(mp));

		//循环开始

		for (int j = 0; j < A.size(); j++)

		{

			if (mp\[A[j]]++ == 0)

\--k;

			while (k < 0)

				if (--mp\[A[i++]] == 0) ++k; //k为0时就将滑动窗口的左侧左移

			ans += j - i + 1; 	//以当前滑动窗口为最右侧的字符串为end的字串的数量

		}

		return ans;

	};

	return sub(k) - sub(k-1);

}



\`\``
