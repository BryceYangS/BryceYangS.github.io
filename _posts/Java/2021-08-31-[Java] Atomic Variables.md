---
layout: post
title: "[Java] Atomic"
subtitle: "atomic"
categories: study
tags: java
---

> Java에서의 Atomic

## 자바 동시성 문제 해결 방법 3가지
1. volatile  
Thread1 에서 쓰고, Thread2에서 읽는 경우에만 동시성 보장. 오직 한 개의 쓰레드에서 쓰기 작업을 할 때, 다른 쓰레드는 읽기 작업만 할 때 안정성을 보장한다. 두 개의 쓰레드에서 쓰기를 할 경우 문제 발생할  수 있음.  

2. synchronized  
안전한 동시성 보장 가능. 다만 비용이 가장 크다는 단점이 있음.  

3. Atomic  
Comapare-and-swap 알고리즘 사용 동시성 보장. 여러 쓰레드에서 데이터 write해도 문제 없음.

## CAS(compare-and-swap)
멀티 쓰레드 환경, 멀티 코어 환경에서 각 CPU는 메인 메모리에서 변수값을 참조하는게 아닌, 각 CPU의 캐시영역에서 메모리 값을 참조하게 된다. 이 때 메인 메모리에 저장된 값과 CPU 캐시에 저장된 값이 다른 경우가 있다.(이를 *가시성 문제*라고 함) 그래서 사용되는 것이 CAS 알고리즘. 현재 쓰레드에 저장된 값과 메인 메모리에 저장된 값을 비교해 일치하는 경우 새로운 값으로 교체, 일치 하지 않는 다면 실패하고 재시도를 한다.  
  
synchronized 블락의 경우 synchronized 블락 진입 전후에 메인 메모리와 CPU 캐시 메모리의 값을 동기화하기 때문에 문제가 없도록 처리한다.

## AtomicInteger

atomic 클래스는 `Compare And Swap(CAS)` 알고리즘을 사용. 일반적으로 synchronize를 통한 lock 보다 더 빠름.  

💡 원자적(Atomic)? `synchronized` 키워드나 `lock`을 사용하지 않고도 멀티 쓰레드로 병렬 연산을 안전하게 실행할 수 있을 때를 가리킴.  


```java
AtomicInteger atomicInt = new AtomicInteger(0);
ExecutorService executor = ExeCutors.newFixedThreadPool(2);

IntStream.range(0, 1000)
    .forEach(i -> executor.submit(atomicInt::incrementAndGet));

Stop(executor);

System.out.println(atomicInt.get()); // 1000
```  

Integer 대신에 AtomicInteger를 사용하면 변수에 대한 sychronize 없이 Thread-safe하게 사용할 수 있음.  
`incrementAndGet()` 메소드는 atomic(원자성)하게 작동. 따라서 멀티 쓰레드에서 해당 메소드를 안전하게 사용할 수 있음.  

<br/>

AtomicInteger는 다양한 종류의 원자성 메소드를 지원함. `updateAndGet()`은 정수에 대해 산술 연산을 수행하기 위해 람다 표현식을 수용함.

```java
AtomicInteger atomicInt = new AtomicInteger(0);

ExecutorService executor = Executors.newFixedThreadPool(2);

IntStream.range(0, 1000)
    .forEach(i -> {
        Runnable task = () -> 
            atomicInt.updateAndGet(n -> n + 2);
        executor.submit(task);
    });

stop(executor);

System.out.println(atomicInt.get()); // 2000
```



## Reference
- [Java 8 Concurrency Tutorial: Atomic Variables and ConcurrentMap](https://winterbe.com/posts/2015/05/22/java8-concurrency-tutorial-atomic-concurrent-map-examples/)
- [https://javaplant.tistory.com/23](https://javaplant.tistory.com/23)