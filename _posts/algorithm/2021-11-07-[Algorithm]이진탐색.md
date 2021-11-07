---
layout: post
title: "[Algorithm] 이진 탐색 구현"
subtitle: "이진 탐색 구현"
categories: study
tags: algo
---


## 이진 탐색
- 정렬된 리스트를 전제로 함.
- 1회 비교를 거칠 때마다 탐색 범위가 절반으로 줄어듦
- 시간복잡도 : O(log N)

```python
def binary_search(element, some_list):
    start_index = 0
    end_index = len(some_list) - 1
    
    while start_index <= end_index:
        mid_index = (start_index + end_index) // 2
        if some_list[mid_index] == element:
            return mid_index
        elif some_list[mid_index] < element:
            start_index = mid_index + 1
        elif some_list[mid_index] > element:
            end_index = mid_index - 1
        
    return None
```