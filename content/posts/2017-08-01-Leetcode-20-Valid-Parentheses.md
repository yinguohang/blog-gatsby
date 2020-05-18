---
title: "Leetcode 20 Valid Parentheses"  
date: "2017-08-01 23:35:00 +0800"  
template: "post"
draft: false
slug: "leetcode-20"
category: "leetcode"
description: "Solution for Leetcode 20" 
tags: 
  - "C++"
---
[Question URL](https://leetcode.com/problems/valid-parentheses/description/)  

### 题目大意
判断(){}[]是否合理

### 解题思路
使用一个栈来存左括号，在遇到右括号的时候与栈顶括号进行配对，如果成功则退栈，否则返回false；如果遇到右括号时栈空，返回false；如果扫描到最后栈是空的，返回true。

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(' || s[i] == '[' || s[i] == '{') 
                st.push(s[i]);
            else if (s[i] == ')' || s[i] == ']' || s[i] == '}') {
                if (st.size() == 0)
                    return false;
                if (abs(s[i] - st.top()) <= 2)
                    st.pop();
                else
                    return false;
            }
        }
        if (st.size() == 0)
            return true;
        return false;
    }
};
```
