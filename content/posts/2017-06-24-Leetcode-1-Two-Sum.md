---
title: "Leetcode 1 Two Sum"  
date: "2017-06-24 18:39:00 +0800"
template: "post"
draft: false
slug: "leetcode-1"
category: "leetcode"
description: "Solution for Leetcode 1 Two Sum"
tags:
  - "C++"
---
[Question URL](https://leetcode.com/problems/two-sum/#/description)  

### 题目大意
给定一个数列，求得两个和为某个值(target)的元素的下标。  
保证解唯一，并且同一个元素不可以被使用两次。  

### 解题思路
维护一个map<int, int>，其中key是数列中元素的值，value是数列中元素的下标。  
从前往后遍历数列中每一个元素，如果target-当前元素在map中不存在，则将其加入map，否则输出它的下标以及它的另一半的下标。  
时间复杂度为O(nlog(n))，将map换为unordered_map可以变为O(n)。  

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> m;
        for (int i = 0; i < nums.size(); i++) {
            if (m.find(target - nums[i]) != m.end()) {
                vector<int> ans;
                ans.push_back(m[target - nums[i]]);
                ans.push_back(i);
                return ans;
            }
            m[nums[i]] = i;
        }
    }
};
```
