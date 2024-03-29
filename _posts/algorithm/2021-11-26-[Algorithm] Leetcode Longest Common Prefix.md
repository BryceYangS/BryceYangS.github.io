---
layout: post
title: "[Algorithm] Leetcode Longest Common Prefix"
subtitle: "Longest Common Prefix"
categories: study
tags: algo
---
> Leetcode Longest Common Prefix

[https://leetcode.com/problems/longest-common-prefix/](https://leetcode.com/problems/longest-common-prefix/)  


## 1. 나의 풀이

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        
        if (strs.length == 1){
            return strs[0];
        }
        
        Arrays.sort(strs, (e1, e2) -> e1.length() - e2.length());
        
        int prefix_index = -1;
        for(int i = 1; i < strs[0].length() + 1; i++) {
            String prefix_candidate = strs[0].substring(0, i);
            boolean isPrefix = true;
            for (int j = 1; j < strs.length; j++) {
                if (!strs[j].startsWith(prefix_candidate)) {
                    isPrefix = false;
                    break;
                }
            }
            
            if (!isPrefix) {
                break;
            }
            prefix_index = i;
        }
        
        if (prefix_index == -1){
            return "";
        }
        
        return strs[0].substring(0, prefix_index);
        
    }
}
```


## 2. Solution - Horizontal scanning
```java
public String longestCommonPrefix(String[] strs) {
    if (strs.length == 0) return "";
    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++)
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) return "";
        }        
    return prefix;
}
```


## 3. Solution - Vertical scanning
```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    for (int i = 0; i < strs[0].length() ; i++){
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j ++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c)
                return strs[0].substring(0, i);             
        }
    }
    return strs[0];
}
```

## 4. Solution - Divide and conquer
```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";    
        return longestCommonPrefix(strs, 0 , strs.length - 1);
}

private String longestCommonPrefix(String[] strs, int l, int r) {
    if (l == r) {
        return strs[l];
    }
    else {
        int mid = (l + r)/2;
        String lcpLeft =   longestCommonPrefix(strs, l , mid);
        String lcpRight =  longestCommonPrefix(strs, mid + 1,r);
        return commonPrefix(lcpLeft, lcpRight);
   }
}

String commonPrefix(String left,String right) {
    int min = Math.min(left.length(), right.length());       
    for (int i = 0; i < min; i++) {
        if ( left.charAt(i) != right.charAt(i) )
            return left.substring(0, i);
    }
    return left.substring(0, min);
}
```

## 5. Solution - Binary search
```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0)
        return "";
    int minLen = Integer.MAX_VALUE;
    for (String str : strs)
        minLen = Math.min(minLen, str.length());
    int low = 1;
    int high = minLen;
    while (low <= high) {
        int middle = (low + high) / 2;
        if (isCommonPrefix(strs, middle))
            low = middle + 1;
        else
            high = middle - 1;
    }
    return strs[0].substring(0, (low + high) / 2);
}

private boolean isCommonPrefix(String[] strs, int len){
    String str1 = strs[0].substring(0,len);
    for (int i = 1; i < strs.length; i++)
        if (!strs[i].startsWith(str1))
            return false;
    return true;
}
```