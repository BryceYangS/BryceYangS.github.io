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


- 답안
```java
class Solution {
    public List<Integer> addToArrayForm(int[] A, int K) {
        int N = A.length;
        int cur = K;
        List<Integer> ans = new ArrayList();

        int i = N;
        while (--i >= 0 || cur > 0) {
            if (i >= 0)
                cur += A[i];
            ans.add(cur % 10);
            cur /= 10;
        }

        Collections.reverse(ans);
        return ans;
    }
}
```



- [https://leetcode.com/problems/add-to-array-form-of-integer/](https://leetcode.com/problems/add-to-array-form-of-integer/)