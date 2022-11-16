---
layout: post
title: "LeetCode_100Hot_day1"
date: 2022-11-16
description: "DAY1"
tag: LeetCode
author: bugthree
---


### day1 {Topic2: 两数相加}
<details>
<summary>题目描述</summary>
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.

示例 2：

输入：l1 = [0], l2 = [0]
输出：[0]

示例 3：

输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]

提示：

    每个链表中的节点数在范围 [1, 100] 内
    0 <= Node.val <= 9
    题目数据保证列表表示的数字不含前导零。
</details>

[题目链接](https://leetcode.cn/problems/add-two-numbers)

#### 思路
首先按照题目需要建立一个链表，如代码中ListNode，首先考虑的是将两个链表对齐，短的以0补齐，然后循环取两个链表的每一个节点相加，x+y，要考虑到有进位的情况(对进位的处理carry = s // 10)，因此对应节点 相加为x+y+carry，将结果赋值给新的链表(也要考虑进位的问题)

```dotnetcli
# Definition for singly-linked list
class ListNode():

    def __init__(self, val):
        if isinstance(val, int):
            self.val = val
            self.next = None

        elif isinstance(val, list):
            self.val = val[0]
            self.next = None
            cur = self
            for i in val[1:]:
                cur.next = ListNode(i)
                cur = cur.next


class Solution:

    def addTwoNumbers(self, l1: Optional[ListNode],
                      l2: Optional[ListNode]) -> Optional[ListNode]:
        re = ListNode(0)  # 创建一个空链表来保存结果
        r = re  # r指向首指针
        carry = 0  # 创建一个变量来保存进位的结果
        while (l1 or l2):
            x = l1.val if l1 else 0
            y = l2.val if l2 else 0
            s = x + y + carry
            carry = s // 10
            r.next = ListNode(s % 10)  # 首指针的next值为s%0
            r = r.next  # r指向第下一个值
            if (l1 != None): l1 = l1.next  # 移向下一个结点取值
            if (l2 != None): l2 = l2.next
        # 循环结束之后，注意如果还有进位需要保存
        if (carry > 0):
            r.next = ListNode(1)
        return re.next
```

测试代码：
```dotnetcli
if __name__ == "__main__":
    a = Solution()
    l1 = ListNode([2, 4, 3])
    l2 = ListNode([5, 6, 4])
    print(a.addTwoNumbers(l1, l2))
```