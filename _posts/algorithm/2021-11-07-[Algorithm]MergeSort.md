---
layout: post
title: "[Algorithm] 합병 정렬(Merge Sort)"
subtitle: "합병 정렬(Merge Sort)"
categories: study
tags: algo
---
> 합병 정렬 구현

- Divide and Conquer 활용
- Divide : 리스트를 반으로 나눔
- Conquer : 왼쪽 리스트와 오른쪽 리스트를 각각 정렬
- Combine : 정렬된 두 리스트를 하나의 정렬된 리스트로 `합병`
     - Combine 과정에서 나뉜 각 리스트의 값을 비교해서 `정렬된 결과의 리스트`로 병합되도록 해야함

```python
def merge(list1, list2):
    merged_list = list()
    if len(list1) == 0:
        return list2

    while list1:
        left = list1.pop(0)
        while list2:
            right = list2[0]
            if left < right:
                break
            merged_list.append(list2.pop(0))
        merged_list.append(left)

    if len(list2) > 0:
        merged_list += list2

    return merged_list

def merge_sort(my_list):
    # base case
    if len(my_list) < 2:
        return my_list

    # my_list를 반씩 나눈다(divide)
    left_half = my_list[:len(my_list)//2]    # 왼쪽 반
    right_half = my_list[len(my_list)//2:]   # 오른쪽 반

    # merge_sort 함수를 재귀적으로 호출하여 부분 문제 해결(conquer)하고,
    # merge 함수로 정렬된 두 리스트를 합쳐(combine)준다
    return merge(merge_sort(left_half), merge_sort(right_half))
```

- 다른 구현 코드

```python
def merge(list1, list2):
    i = 0
    j = 0

    # 정렬된 항목들을 담을 리스트
    merged_list = []

    # list1과 list2를 돌면서 merged_list에 항목 정렬
    while i < len(list1) and j < len(list2):
        if list1[i] > list2[j]:
            merged_list.append(list2[j])
            j += 1
        else:
            merged_list.append(list1[i])
            i += 1

    # list2에 남은 항목이 있으면 정렬 리스트에 추가
    if i == len(list1):
        merged_list += list2[j:]

    # list1에 남은 항목이 있으면 정렬 리스트에 추가
    elif j == len(list2):
        merged_list += list1[i:]

    return merged_list

def merge_sort(my_list):
    # base case
    if len(my_list) < 2:
        return my_list

    # my_list를 반씩 나눈다(divide)
    left_half = my_list[:len(my_list)//2]    # 왼쪽 반
    right_half = my_list[len(my_list)//2:]   # 오른쪽 반

    # merge_sort 함수를 재귀적으로 호출하여 부분 문제 해결(conquer)하고,
    # merge 함수로 정렬된 두 리스트를 합쳐(combine)준다
    return merge(merge_sort(left_half), merge_sort(right_half))
```