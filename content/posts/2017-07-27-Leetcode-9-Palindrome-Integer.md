---
title: "Leetcode 9 Palindrome Integer"  
date: "2017-07-27 21:43:00 +0800"
template: "post"
draft: false
slug: "leetcode-9"
category: "leetcode"
description: "Solution for Leetcode 9 Palindrome Integer"
tags:
  - "C++"
---
[Question URL](https://leetcode.com/problems/palindrome-number/tabs/description)  

### 题目大意
判断一个int(32 bit)是否是回文数。

### 解题思路
同Leetcode 7，注意负数不是回文数。

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return false;
        long long n = 0;
        int tmp = x;
        while (tmp != 0) {
            n = n * 10 + tmp % 10;
            tmp = tmp / 10;
        }
        if (n == (long long)x) {
            return true;
        }
        else 
            return false;
    }
};
```
