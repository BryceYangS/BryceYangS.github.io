---
layout: post
title: "[Algorithm] Leetcode Roman to Integer"
subtitle: "Roman to Integer"
categories: study
tags: algo
---
> Leetcode Roman to Integer

[https://leetcode.com/problems/roman-to-integer/](https://leetcode.com/problems/roman-to-integer/)  


## 1. 나의 풀이

```java
class Solution {
    public int romanToInt(String s) {
		if (s.length() == 1) {
			return convertToint(s.charAt(0));
		}

		int sum = 0;
		while (s.length() > 1) {
			if (isMinus(s.charAt(0), s.charAt(1))) {
				sum -= convertToint(s.charAt(0));
			} else {
				sum += convertToint(s.charAt(0));
			}
			s = s.substring(1);
		}

		sum += convertToint(s.charAt(0));

		return sum;
	}

	private int convertToint(char ch) {
		switch (ch) {
			case 'I':
				return 1;
			case 'V':
				return 5;
			case 'X':
				return 10;
			case 'L':
				return 50;
			case 'C':
				return 100;
			case 'D':
				return 500;
			case 'M':
				return 1000;
		}
		return 0;
	}

	private boolean isMinus(char beforeChar, char afterChar) {
		if (beforeChar == 'I') {
			if (afterChar == 'V' || afterChar == 'X') {
				return true;
			}
		}

		if (beforeChar == 'X') {
			if (afterChar == 'L' || afterChar == 'C') {
				return true;
			}
		}

		if (beforeChar == 'C') {
			if (afterChar == 'D' || afterChar == 'M') {
				return true;
			}
		}

		return false;
	}
}
```