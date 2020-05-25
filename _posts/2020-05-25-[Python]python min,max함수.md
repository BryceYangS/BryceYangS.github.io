---
layout: post
title: "[Python] enumerate(), min(), max() 함수"
subtitle: "python min, max 함수"
categories: study
tags: python
---

## min, max 함수
```python
students = ['bryce', 'allen', 'shelden', 'bob']

# 제일 긴 학생의 이름
longest_name = max(students, key= lambda n: len(n))

print(longest_name)
# shelden
```


1. max() 
 - 객체에서 가장 큰 항목 또는 두 개 이상 arg 중 가장 큰 값 리턴
```python
a = [1,2,3,4]
print(max(a))
# 4 
```  
```python
a = {1:10, 2:-1, -10:9}

print(max(a))
# 2

print(max(a, key=lambda x: a[x]))
# 1
```

2. enumerate()
 - 객체의 인덱스를 추가해 리턴. 추가한 형태는 tuple 형태로 리턴

```python
a = [1,2,3,4]

tmp = [(idx,i) for idx, i in enumerate(a)]
print(tmp)
# [(0, 1), (1, 2), (2, 3), (3, 4)]

#-------------------------------
a = ['a','b','c','d']

tmp = list(enumerate(a))
print(tmp)
# [(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd')]

#------------------------------
a = ['a','b','d','c']

tmp = list(enumerate(a))

print(max(tmp, key=lambda x:x[1]))
# (2,'d')
```