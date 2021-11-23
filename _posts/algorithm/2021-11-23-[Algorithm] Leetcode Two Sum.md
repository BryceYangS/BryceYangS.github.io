---
layout: post
title: "[Algorithm] Leetcode Two Sum"
subtitle: "Two Sum"
categories: study
tags: algo
---
> Leetcode Two Sum

[https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)  


## Brute Force
- 시간 복잡도 : O(n)
- 공간 복잡도 : O(1)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length - 1; i++) {
			int sum = nums[i];
			for (int j = i + 1; j < nums.length; j++) {
				if (sum + nums[j] == target) {
					return new int[] {i, j};
				}
			}
		}
		return null;
    }
}
```

## Two-pass Hash Table
- 시간 복잡도 : O(n)
- 공간 복잡도 : O(n)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
		Map<Integer, Integer> map = new HashMap<>();

		for (int i = 0; i < nums.length; i++) {
			map.put(nums[i], i);
		}

		for (int i = 0; i < nums.length; i++) {
			int complement = target - nums[i];
			if (map.containsKey(complement) && map.get(complement) != i) {
				return new int[] {i, map.get(complement)};
			}
		}
		return null;
    }
}
```

## One-pass Hash Table
- 시간 복잡도 : O(n)
- 공간 복잡도 : O(n)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
		Map<Integer, Integer> map = new HashMap<>();
		for (int i = 0; i < nums.length; i++) {
			int complement = target - nums[i];
			if (map.containsKey(complement)) {
				return new int[] {map.get(complement), i};
			}
			map.put(nums[i], i);
		}
		return null;
    }
}
```