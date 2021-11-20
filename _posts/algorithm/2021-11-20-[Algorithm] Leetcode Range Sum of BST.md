---
layout: post
title: "[Algorithm] Leetcode Range Sum of BST"
subtitle: "Range Sum of BST"
categories: study
tags: algo
---
> Leetcode Range Sum of BST


## BFS 활용
```java
class Solution {
    public int rangeSumBST(TreeNode root, int low, int high) {
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        
        int sum = 0;
        int qlen = 0;
        while(q.size() > 0) {
            qlen = q.size();
            for(int i = 0; i < qlen; i++) {
                TreeNode curr = q.poll();
                if (curr.val >= low && curr.val <= high) {
                    sum += curr.val;
                }
                
                if (curr.left != null) {
                    q.add(curr.left);
                }
                
                if (curr.right != null) {
                    q.add(curr.right);
                }
            } 
        }
        
        return sum;
    }
}```

## DFS 활용
```java
class Solution {
    
    int sum = 0;
    
    public int rangeSumBST(TreeNode root, int low, int high) {
        
        dfs(root, low, high);
        
        return sum;
    }
    
    
    private void dfs(TreeNode node, int low, int high) {
        if (node.val >= low && node.val <= high) {
            sum += node.val;
        }
        
        if (node.left != null) {
            dfs(node.left, low, high);
        }
        
        if (node.right != null) {
            dfs(node.right, low, high);
        }
    }
}
```

- [https://leetcode.com/problems/range-sum-of-bst/submissions/](https://leetcode.com/problems/range-sum-of-bst/submissions/)