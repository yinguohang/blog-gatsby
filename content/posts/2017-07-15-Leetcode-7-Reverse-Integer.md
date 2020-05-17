---
title: "Leetcode 7 Reverse Integer"  
date: "2017-07-27 20:00:00 +0800"
template: "post"
draft: false
slug: "leetcode-7"
category: "leetcode"
description: "Solution for Leetcode 7 Reverse Integer"
tags:
  - "C++"
---
[Question URL](https://leetcode.com/problems/reverse-integer/#/description)  

### 题目大意
给定一个32位有符号数，返回将其10进制翻转过来显示的数字，如果溢出，则返回0。

### 解题思路
这道题的关键主要在于如何判断是否溢出，可以使用long long类型(64 bit)来解决这一问题。ans为一个long long类型的变量，只需要判断 ans == (long long)(int)ans 即可判断是否存在溢出问题。

```cpp
class Solution {
public:
    int reverse(int x) {
        long long ans = 0;
        int sign = x < 0 ? -1 : 1;
        x = x < 0 ? -x : x;
        while (x != 0) {
            ans = ans * 10 + x % 10;
            x = x / 10;
        }
        ans = ans * sign;
        if (ans == (long long)(int)ans) {
            return int(ans);
        } else {
            return 0;
        }
    }
};
```
