---
layout: post
title: "[Algorithm] Leetcode Add Strings"
subtitle: "Add Strings"
categories: study
tags: algo
---
> Leetcode Add Strings

```java
public class Solution415 {
	public String addStrings(String num1, String num2) {
		int i = num1.length()-1;
		int j = num2.length()-1;
		int carry = 0;
		StringBuilder sb = new StringBuilder();

		while(i >= 0 || j >= 0 || carry > 0) {

			int c1 = 0;
			int c2 = 0;

			if(i >= 0) c1 = (int)(num1.charAt(i) - '0');

			if(j >= 0) c2 = (int)(num2.charAt(j) - '0');

			int sum = c1 + c2 + carry;

			carry = sum/10;

			sum = sum%10;

			sb.append(sum);

			i--;
			j--;
		}

		return sb.reverse().toString();
	}
}
```


- [https://leetcode.com/problems/add-strings/](https://leetcode.com/problems/add-strings/)