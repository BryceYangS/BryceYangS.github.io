---
layout: post
title: "[Algorithm] Leetcode Deepest Leaves Sum"
subtitle: "Deepest Leaves Sum"
categories: study
tags: algo
---
> Leetcode Deepest Leaves Sum


## BFS 활용
```java
class Solution {
    public int deepestLeavesSum(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        int ans = 0, qlen = 0;
        while (q.size() > 0) {
            qlen = q.size();
            ans = 0;
            for (int i = 0; i < qlen; i++) {
                TreeNode curr = q.poll();
                ans += curr.val;
                if (curr.left != null) q.add(curr.left);
                if (curr.right != null) q.add(curr.right);
            }
        }
        return ans;            
    }
}
```

## DFS 활용
```java
class Solution {
    
    int maxLevel = 0;
    int sum = 0;
    
    public int deepestLeavesSum(TreeNode root) {
        int level = 0;

        if (root != null) {
            dfs (root, level);
        }

        return sum;
    }
    
    private void dfs(TreeNode node, int level) {

        if (level  > maxLevel) {
            sum = 0;
            maxLevel = level;
        }

        if (level == maxLevel) {
            sum += node.val;
        }

        if (node.right != null) {
            dfs(node.right, level+1);            
        }

        if (node.left != null) {
            dfs(node.left, level+1);
        }
    }
}
```



- [https://leetcode.com/problems/deepest-leaves-sum/](https://leetcode.com/problems/deepest-leaves-sum/)