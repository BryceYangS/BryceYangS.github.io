---
layout: post
title: "[Java] Stream Distinct 적용 방법"
subtitle: "stream distinct"
categories: study
tags: java
---

> Stream을 활용한 클래스 속성 멀티건 기준 Distinct 적용

```java
public static <T> Predicate<T> distinctByKeys(Function<? super T, ?>... keyExtractors){
    final Map<List<?>, Boolean>seen = new ConcurrentHashMap<>();

    return t -> {
        final List<?> keys = Arays.stream(keyExtractors).map(ke -> ke.apply(t)).collect(Collectors.toList());

        return Ojects.isNull(seend.putIfAbsent(keys, Boolean.TRUE));
    }
}
```

활용방법

```java
List<TestData> dintinctedData = testData.stream()
    .filter(distinctByKeys(TestData::getId, TestData::getName))
    .collect(Collectors.toList());
```
