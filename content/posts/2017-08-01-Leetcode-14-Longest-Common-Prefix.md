---
title: "Leetcode 14 Longest Common Prefix"  
date: "2017-08-01 23:21:00 +0800"  
template: "post"
draft: false
slug: "leetcode-14"
category: "leetcode"
description: "Solution for Leetcode 14" 
tags:
  - "C++"
---
[Question URL](https://leetcode.com/problems/longest-common-prefix/description/)  

### 题目大意
寻找字符串数组中的公共字符串前缀。

### 解题思路

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 0) 
            return "";
        int l = strs[0].size();
        for (int i = 1; i < strs.size(); i++) 
            if (strs[i].size() < l) 
                l = strs[i].size();
        int ans = 0;
        for (int i = 0; i < l; i++) {
            for (int j = 1; j < strs.size(); j++) {
                if (strs[j][i] != strs[0][i])
                    return strs[0].substr(0, ans);
            }
            ans++;
        }
        return strs[0].substr(0, ans);
    }
};
```
