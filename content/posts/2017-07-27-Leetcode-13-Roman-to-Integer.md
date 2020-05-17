---
title: "Leetcode 13 Roman to Integer"  
date: "2017-07-27 21:59:00 +0800"
template: "post"
draft: false
slug: "leetcode-13"
category: "leetcode"
description: "Solution for Leetcode 13 Roman to Integer"  
tags:
  - "C++"
---
[Question URL](https://leetcode.com/problems/roman-to-integer/tabs/description)  

### 题目大意
将一个字符串类型的罗马数字转换为整数类型。输入的范围在1和3999之间。

[罗马数字-中文介绍-百度百科](https://baike.baidu.com/item/%E7%BD%97%E9%A9%AC%E6%95%B0%E5%AD%97/772296?fr=aladdin#3)
规则如下：
1. 罗马数字中的数字符号有7种：I(1)、V(5)、X(10)、L(50)、C(100)、D(500)、M(1000)。
2. 一个罗马数字重复几次，就表示这个数的几倍。但同一数码不能出现三次以上。
3. 左减右加
4. 左减的数字有限制，仅限于I、X、C。
5. 左减时不可跨越一个位数。比如，99不可以用IC（100 - 1）表示。

### 解题思路
罗马数字实际上为逐个表示每个数字（因为不允许跨越数位）。
在表示每个数位的时候，其构成元素（数字符号）的结构为一个[小]/[(小-)中(-小)]/[小-大]的结构，比如在表示百位的时候会用到C(100)、D(500)、M(1000)，其中C为[小]，D为[中]，M为[大]；而高位的最小数字符号大于等于低位的最大数字符号，因此不存在连续的数字符号表示不同的位数，所以处理相同的连续数字符号的时候可以将它们合并成一个变量(cur)；即表示高位的最后一个数字符号肯定大于表示相邻的地位的第一个数字符号，因此上升的结构只存在于表示一个数位的时候（[小-中/大]）而不会出现在分别表示两个数位的结构中。
因此从左向右扫描，构成整个罗马数字的数字符号在
1. 变大的时候需要减去cur即可，之后清空cur。
2. 不变的时候累计cur。
3. 变小的时候加上当前cur，重新累计cur。

```cpp
class Solution {
public:
    int romanToInt(string s) {
        if (s.size() == 0) return 0;
        map<char, int> dict;
        dict['I'] = 1;
        dict['V'] = 5;
        dict['X'] = 10;
        dict['L'] = 50;
        dict['C'] = 100;
        dict['D'] = 500;
        dict['M'] = 1000;
        int ans = 0;
        int cur = dict[s[0]];
        for (int i = 1; i < s.size(); i++) {
            if (s[i] == s[i - 1])
                cur = cur + dict[s[i]];
            else if (dict[s[i]] > dict[s[i - 1]]) {
                ans = ans + dict[s[i]] - cur;    
                cur = 0;
            } else {
                ans = ans + cur;
                cur = dict[s[i]];
            }
        }
        return ans + cur;
    }
};
```
