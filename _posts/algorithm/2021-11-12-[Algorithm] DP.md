---
layout: post
title: "[Algorithm] Dynamic Programming"
subtitle: "Dynamic Programming"
categories: study
tags: algo
---
> Dynamic Programming

## Dynamic Programming?
> 한 번 계산한 결과를 재활용하는 방식

## Dynamic Programming의 조건
- 최적 부분 구조 (Optimal Substructure)
- 중복되는 부분 문제 (Overlapping Subproblems)


### 최적 부분 구조 (Optimal Substructure)
- 부분 문제들의 최적의 답을 이용해서 기존 문제의 최적의 답을 구할 수 있다는 것


### Dynamic Programming 적용 조건
1. 최적 부분 구조가 있다
2. 중복되는 부분 문데들이 있다


### 구현 방법 2가지
1. Memoization
2. Tabulation

### Memoization
- 중복되는 계산은 한 번만 계산 후 메모
- 재귀

#### 피보나치 Memoization 활용 코드
- 하향식 접근

```python
def fib_memo(n, cache):
    # base case
    if n < 3:
        return 1
        
    # 이미 n번째 피보나치를 계산했으면:
    # 저장된 값을 바로 리턴한다
    if n in cache:
        return cache[n]
    
    # 아직 n번째 피보나치 수를 계산하지 않았으면:
    # 계산을 한 후 cache에 저장
    cache[n] = fib_memo(n - 1, cache) + fib_memo(n - 2, cache)

    # 계산한 값을 리턴한다
    return cache[n]

def fib(n):
    # n번째 피보나치 수를 담는 사전
    fib_cache = {}

    return fib_memo(n, fib_cache)
```


### Tabulation
- 상향식 접근
- 반복문

#### 피보나치 Tabulation 활용 코드

```python
def fib_tab(n):
    # 이미 계산된 피보나치 수를 담는 리스트
    fib_table = [0, 1, 1]

    # n번째 피보나치 수까지 리스트를 하나씩 채워 나간다
    for i in range(3, n + 1):
        fib_table.append(fib_table[i - 1] + fib_table[i - 2])

    # 피보나치 n번째 수를 리턴한다
    return fib_table[n]
def fib_tab(n):
    # 이미 계산된 피보나치 수를 담는 리스트
    fib_table = [0, 1, 1]

    # n번째 피보나치 수까지 리스트를 하나씩 채워 나간다
    for i in range(3, n + 1):
        fib_table.append(fib_table[i - 1] + fib_table[i - 2])

    # 피보나치 n번째 수를 리턴한다
    return fib_table[n]
```

- 위의 방식은 시간복잡도 O(n) / 공간복잡도 O(n)


```python
def fib_optimized(n):
    current = 1
    previous = 0

    # 반복적으로 위 변수들을 업데이트한다. 
    for i in range(1, n):
        current, previous = current + previous, current

    # n번재 피보나치 수를 리턴한다. 
    return current
```

- 위의 경우 공간복잡도 O(1)

## Memoization VS Tabulation
- 공통점
    - 둘 다 중복되는 부분 문제의 비효율을 해결
- 차이점
    - Memoization : 재귀 사용
    - Tabulation : 반복문 사용

### Memoization
- 재귀 호출로 인한 스택오버플로우 에러 발생할 수 있음
- 위에서부터 계산하기 때문에 필요 없는 계산은 생략할 수 있음 -> Tabulation은 모든 값을 구해 나가야 함

### Tabulation
- 반복문 사용이기 때문에 memoization의 호출 스택 에러 발생하지 않음
- 모든 부분 문제를 해결하기 때문에 모든 부분 문제가 중요할 때 사용