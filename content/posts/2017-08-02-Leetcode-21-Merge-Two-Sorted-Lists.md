---
title: "Leetcode 21 Merge Two Sorted Lists"  
date: "2017-08-02 00:23:00 +0800"  
template: "post"
draft: false
slug: "leetcode-21"
category: "leetcode"
description: "Solution for Leetcode 21" 
tags:
  - "C++"
---
[Question URL](https://leetcode.com/problems/merge-two-sorted-lists/description/)  

### 题目大意
合并两个有序链表

### 解题思路

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL)
            return l2;
        if (l2 == NULL)
            return l1;
        ListNode *head, *tail, *ans;
        head = new ListNode(0);
        tail = head;
        ListNode* p1 = l1; 
        ListNode* p2 = l2;
        while (p1 != NULL && p2 != NULL) {
            if (p1 -> val < p2 -> val) {
                tail -> next = p1; 
                tail = p1;
                p1 = p1 -> next;
            } else {
                tail -> next = p2;
                tail = p2;
                p2 = p2 ->next;
            }
        }
        if (p1 == NULL)
            tail -> next = p2;
        else
            tail -> next = p1;
        ans = head -> next;
        delete head;
        return ans;
    }
};
```
