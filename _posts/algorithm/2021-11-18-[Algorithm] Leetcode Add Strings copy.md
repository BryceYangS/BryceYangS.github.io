---
layout: post
title: "[Algorithm] Leetcode Add to Array-Form of Integer"
subtitle: "Add to Array-Form of Integer"
categories: study
tags: algo
---
> Leetcode Add to Array-Form of Integer

```java
public class Solution989 {
	public List<Integer> addToArrayForm(int[] num, int k) {

		int l1 = num.length - 1;
		int carry = 0;

		List<Integer> rtn = new ArrayList<>();

		while (l1 >= 0 || k > 0 || carry > 0) {

			int arrNum = 0;
			if (l1 >= 0) {
				arrNum = num[l1];
			}

			int sum = arrNum + k % 10 + carry;

			carry = sum / 10;

			k /= 10;

			rtn.add(sum % 10);

			l1--;
		}

		Collections.reverse(rtn);
		return rtn;
	}
}
```


- [https://leetcode.com/problems/add-to-array-form-of-integer/](https://leetcode.com/problems/add-to-array-form-of-integer/)