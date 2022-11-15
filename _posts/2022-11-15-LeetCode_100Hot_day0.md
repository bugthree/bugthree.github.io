---
layout: post
title: "LeetCode_100Hot_day0"
date: 2022-11-15
description: "DAY0"
tag: LeetCode
author: bugthree
---

[toc]

## LeetCode hot 100 
### day0 {Topic1: 两数之和}

<details>
<summary>题目描述</summary>
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

示例 2：

输入：nums = [3,2,4], target = 6
输出：[1,2]

示例 3：
输入：nums = [3,3], target = 6
输出：[0,1]

提示：
    2 <= nums.length <= 104
    -109 <= nums[i] <= 109
    -109 <= target <= 109
    只会存在一个有效答案

进阶：你可以想出一个时间复杂度小于 O(n2) 的算法吗？
</details>

#### 解法1
这个题目比较简单，刚开始的思路也简单的想到了，求得所有两两数字之和，与target 做比较，这种暴力解法虽然思路简单，但是其时间复杂度O(N2),循环的思路是: 外循环假设指向数组的某一个值，则内循环就相当于将其后面的所有值依次与该指向的值相加。如图1-1所示
![](./100dayImg/day0_1-1.png)

```

class Solution:

    def twoSum(self, nums: List[int], target: int) -> List[int]:
        length = len(nums)
        for i in range(length):
            for j in range(i + 1, length):
                if (nums[i] + nums[j] == target):
                    return [i, j]

```

#### 解法2
并没有想出一个时间复杂度小于 O(n2) 的算法，看了解析的答案，恍然大悟，可以使用一个哈希表，去判断target - 数组的值是否在哈希表中，如果击中就返回，此时的间复杂度O(N)

```

class Solution2:

    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashtable = dict()
        # enumerate 是 对于一个可迭代的（iterable）/可遍历的对象（如列表、字符串），enumerate将其组成一个索引序列，利用它可以同时获得索引和值
        # print(i, nums[i])
        for i, num in enumerate(nums):
            if target - num in hashtable:
                return [hashtable[target - num], i]
            hashtable[nums[i]] = i
        return []

```

本地测试代码：

```

if __name__ == "__main__":
    a = Solution()
    print(a.twoSum([3, 2, 4], 6))
    print(a.twoSum([2, 7, 11, 15], 9))
    print(a.twoSum([3, 3], 6))
    a2 = Solution2()
    print(a2.twoSum([3, 2, 4], 6))
    print(a2.twoSum([2, 7, 11, 15], 9))
    print(a2.twoSum([3, 3], 6))

```