---
layout: post
title: "[Algorithm] 퀵 정렬(Quick Sort)"
subtitle: "퀵 정렬(Quick Sort)"
categories: study
tags: algo
---
> 퀵 정렬 구현

## 1. 퀵 정렬이란?

- `기준점(pivot)`을 정해서, 기준점보다 작은 데이터는 왼쪽, 큰 데이터는 오른쪽으로 모으는 함수를 작성
    + 보통 맨 앞 또는 맨 뒤 데이터를 기준점으로 잡음
- 각 왼쪽, 오른쪽은 재귀용법을 사용해서 다시 동일 함수를 호출해 위 작업을 반복
- 함수는 왼쪽 + 기준점 + 오른쪽을 리턴함
- 분할 정복 알고리즘의 대표적인 예 중의 하나

## 2. 알고리즘 구현
- Quick sort 함수 생성
    + 만약 리스트 갯수가 한 개이면 해당 리스트 리턴
    + 그렇지 않으면, 리스트 맨 앞의 데이터를 기준점(pivot)으로 놓기
    + left, right 리스트 변수 생성
    + 맨 앞의 데이터를 뺀 나머지 데이터를 기준점과 비교 
        * 기준점보다 작으면 left.append
        * 기준점보다 크면 right.append
    + return quicksort(left) + pivot + quicksort(ringt)로 재귀호출
    > 리스트로 만들어서 리턴하기 : return quick_sort(left) + [pivot] + quick_sort(right)

```python
def qsort(data):
    if len(data) <= 1:
        return data
    
    left,right = [], []
    pivot = data[0] 
    
    for index in range(1, len(data)):
        if pivot > data[index]:
            right.append(data[index])
        else:
            left.append(data[index])
    
    return qsort(left) + [pivot] + qsort(right)
```

**파이썬 list comprehension 활용**

```python
def qsort(data):
    if len(data) <= 1:
        return data
    
    left,right = [], []
    pivot = data[0] 
    
    left = [item for item in data[1:] if pivot > item]
    right = [item for item in data[1:] if pivot <= item]
    
    return qsort(left) + [pivot] + qsort(right)
```

## 3. 알고리즘 분석
- 병합 정렬과 유사, 시간복잡도는 O(n log n)
    + 단, 최악의 경우
        * 맨 처음 pivot이 가장 크거나, 가장 작으면
        * 모든 데이터를 비교하는 상황이 나옴
        * O(n^2)


## 4. 또 다른 방식으로 구현

```python
# 두 요소의 위치를 바꿔주는 helper function
def swap_elements(my_list, index1, index2):
    temp = my_list[index1]
    my_list[index1] = my_list[index2]
    my_list[index2] = temp


# 퀵 정렬에서 사용되는 partition 함수
def partition(my_list, start, end):
    pivot = end

    i = start
    b = start
    while i < end:
        if my_list[i] < my_list[pivot]:
            swap_elements(my_list, i, b)
            b += 1
        i += 1
    swap_elements(my_list, pivot, b)
    pivot = b
    return pivot


# 퀵 정렬
def quicksort(my_list, start = 0, end=None):
    if end == None:
        end = len(my_list) - 1
        
    if end - start < 1:
        return

    pivot = partition(my_list, start, end)
    quicksort(my_list, start, pivot - 1)
    quicksort(my_list, pivot + 1, end)
```