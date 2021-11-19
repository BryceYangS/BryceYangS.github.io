---
layout: post
title: "[Algorithm] Leetcode Reverse Words in a String III"
subtitle: "Reverse Words in a String III"
categories: study
tags: algo
---
> Leetcode Reverse Words in a String III

```java
class Solution {
    public String reverseWords(String s) {

        String[] s1 = s.split(" ");
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < s1.length; i++) {
            char[] chars = s1[i].toCharArray();

            for (int j = 0; j < chars.length / 2; j++) {
                swap(chars, j, chars.length - j - 1);
            }

            sb.append(new String(chars));
            if (i != s1.length - 1) {
                sb.append(" ");
            }
        }

        return sb.toString();
    }

    private void swap(char[] s2, int j, int i) {
        char tmp = s2[j];
        s2[j] = s2[i];
        s2[i] = tmp;
    }
}
```



- [https://leetcode.com/problems/reverse-words-in-a-string-iii/](https://leetcode.com/problems/reverse-words-in-a-string-iii/)