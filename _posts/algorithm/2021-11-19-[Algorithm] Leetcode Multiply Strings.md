---
layout: post
title: "[Algorithm] Leetcode Multiply Strings"
subtitle: "Multiply Strings"
categories: study
tags: algo
---
> Leetcode Multiply Strings

```java
class Solution {
    public String multiply(String num1, String num2) {
		if ("0".equals(num1) || "0".equals(num2)) {
            return "0";
        }

        if (num1.length() < num2.length()) {
            String tmp = num1;
            num1 = num2;
            num2 = tmp;
        }

        int[] rtnArr = new int[num1.length() + num2.length()];
        int carry = 0;

        int rIdx = rtnArr.length - 1;
        for (int i = num1.length() - 1; i >= 0; i--) {
            int idx = rIdx;
            for (int j = num2.length() - 1; j >= 0 ; j--) {
                int n1 = num1.charAt(i) - '0';
                int n2 = num2.charAt(j) - '0';

                int m = n1 * n2;
                int q = m / 10;
                int r = m % 10 + carry;

                if (r >= 10) {
                    rtnArr[idx] += r % 10;
                    carry = q + 1;
                } else {
                    rtnArr[idx] += r;
                    carry = q;
                }

                if (rtnArr[idx] >= 10) {
                    rtnArr[idx] %= 10;
                    carry += 1;
                }

                idx -= 1;
            }

            if (carry > 0) {
                rtnArr[idx] += carry;
                carry = 0;
            }

            rIdx -= 1;
        }

        StringBuilder sb = new StringBuilder();

        boolean zero = true;
        for (int i = 0; i < rtnArr.length; i++) {
            if (rtnArr[i] != 0) {
                zero = false;
            }

            if (!zero) {
                sb.append(rtnArr[i]);
            }
        }

        if (sb.length() == 0) {
            sb.append("0");
        }

        return sb.toString();
    }
}
```



- [https://leetcode.com/problems/multiply-strings/](https://leetcode.com/problems/multiply-strings/)