---
layout: post
title: "[Interview] 면접 공부"
subtitle: "interview"
categories: study
tags: etc
---
> 면접 공부  

## 1. Java
### HashMap & TreeMap
[[Java] HashMap vs TreeMap](/study/2021/05/24/Java-HashMap-vs-TreeMap)
### primitive & reference / Boxing & Unboxing / Wrapper 끼리 비교
[[Java] Primitive & Reference & Boxing & Unboxing](/study/2021/05/24/Java-Primitive-&-Reference-&-Boxing-&-Unboxing)
### JVM 메모리 & 자바 변수 Scope 3가지 종류 - 저장되는 JVM 메모리 영역 (Local, Instance, Class)
[[Java] JVM Stack & Heap](/study/2021/05/24/Java-JVM-Stack-&-Heap)
### Generic
[[Java]Generics](/study/2020/10/26/Java-Generics)
### GC
[[Java] Garbage Collection](/study/2021/05/14/Java-Garbage-Collection/)
### Java 6-8-11 차이.
[[Java] Java 버전별 스펙](/study/2021/06/07/Java-Java-버전별-스펙)
### JSR 310?.
### 문자열
[[Java] 문자열](/study/2021/06/04/Java-문자열)

### Immutable Object - Java class 중 대표적인 사례
- Collections.unmodifeable

### String vs StringBuffer
- immutable object?

### sychronized?

### java.util.concurrent.ConcurrentHashMap
- 외부에서 동시성을 확보하는 것보다 성능 상 이점 있는 이유
- [[Java] java.util.concurrent](/study/2021/08/16/Java-java.util.concurrent)

### java.util.concurrent.AtomicInteger vs Integer 
[[Java] Atomic Variables](/study/2021/08/31/Java-Atomic-Variables)

### ConcurrentHashMap vs AtomicInteger 방식 차이점
### Heap Dump, Thread Dump
### jar vs war
- jar : java 명령어를 통한 실행 가능
    - 자바 명령어로 -jar 패키징 이름 입력 시 스프링 부트 안에 내장 톰캣 서버가 작동하면서 스프링 부트 기동
    - 자바 파일의 컴포넌트 : 컴포넌트는 배포 단위다. 컴포넌트는 시스템의 구성 요소로 배포할 수 있는 가장 작은 단위*(클린 아키텍처 12장 컴포넌트)
- war
    - web.xml 등 애플리케이션 구조가 web으로 바뀌게 됨. 내장 톰캣을 가지지 않은 상태이기 때문에 외부 톰캣 등 WAS를 사용해 배포하는 방식
    - 여러 컴포넌트를 묶어 단일 아카이브로 생성한 것



## 2. HTTP
### POST & PUT 동일 요청 반복 경우 어떠한 결과가 나오는가
[[HTTP] POST & PUT 반복요청 차이점](/study/2021/05/28/HTTP-POST-&-PUT-반복요청-차이점)
### Redirection
[[HTTP] Redirection](/study/2021/05/28/HTTP-Redirection)
### HTTP - HTTPS 차이.
### http 1.0 / 1.1 / 2.0 차이점


## 3. 네트워크
### Load-Balancer
[[Network] Load Balacing](/study/2021/05/24/Network-Load-Balacing)
### TCP vs UDP
[[Network] TCP & UDP](/study/2021/05/24/Network-TCP-&-UDP)



## 4. 데이터베이스
### Sharding & Replication
[[Database] Sharding & Replication & Clustering](/study/2021/06/01/Database-Sharding-&-Replication-&-Clustering)
### PreparedStatement & Statement
[[Java] Statement & PreparedStatement](/study/2021/05/24/Java-Statement-&-PreparedStatement)
### 복합키
### 정규화/비정규화
### DISTINCT 와 GROUP BY의 차이
- [http://intomysql.blogspot.com/2011/01/distinct-group-by.html](http://intomysql.blogspot.com/2011/01/distinct-group-by.html)
- [https://stackoverflow.com/questions/164319/is-there-any-difference-between-group-by-and-distinct#comment80633329_45833583](https://stackoverflow.com/questions/164319/is-there-any-difference-between-group-by-and-distinct#comment80633329_45833583)


## 5. 스프링
### 커넥션 풀
### @Transactional
### 빈 스코프
### Filter, Interceptor, Spring AOP
### SpringBoot Autoconfiguration 작동방식.
[[SpringBoot] 스프링부트 자동 설정](/study/2021/06/04/SpringBoot-스프링부트-자동-설정)
### Filter / Interceptor 작동 순서

## 6. JPA
### OSIV(Open Session In View)
[[JPA] OSIV](/study/2021/09/01/JPA-OSIV)

## 7. 디자인 패턴
### 전략패턴
[전략패턴 정리 글](https://github.com/BryceYangS/Java/blob/main/DesignPattern/posting/1_Strategy_Pattern.md)
### 옵저버 패턴
[옵저버 패턴 정리 글](https://github.com/BryceYangS/Java/blob/main/DesignPattern/posting/2_Observer.md)
### 상태패턴 vs 전략패턴
- 상태 패턴 : 객체의 내부 상태가 바뀜에 따라서 객체의 행동을 바꿀 수 있습니다. 마치 객체의 클래스가 바뀌는 것과 같은 결과를 얻을 수 있습니다.
    - 상태를 별도의 클래스로 캡슐화한 다음 현재 상태를 나타내는 객체에게 행동을 위임하기 때문에, 내부 상태가 바뀜에 따라서 행동이 달라지게 된다.
- 전략 패턴 : 알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용할 수 있도록만든다.
- 차이점
    - 스테이트 패턴을 사용할 때는 상태 객체에 일련의 행동이 캡슐화된다. 상황에 따라 Context 객체에서 여러 상태 객체 중 한 객체에게 모든 행동을 맡기게 된다. 그 객체의 내부 상태에 따라 현재 상태를 나타내는 객체가 바뀌게 되고, 그 결과로 컨텍스트 객체의 행동도 자연스럽게 바뀌게 된다. `클라이언트는 상태 객체에 대해서 거의 아무것도 몰라도 된다.`
        - `수많은 조건문을 집어넣는 대신`에 사용할 수 있는 패턴
        - 행동을 상태 객체 내에 캡슐화시키면 컨텍스트 내의 상태 객체를 바꾸는 것만으로도 컨텍스트 객체의 행동을 바꿀 수 있음.
        - Context 객체를 생성할 때 초기 상태를 지정해 주는 경우가 있지만, 그 후로는 그 Context 객체가 알아서 자기 상태를 변경. 
    - 스트래티지 패턴을 사용할 때는 일반적으로 클라이언트에서 컨텍스트 객체한테 어떤 전략 객체를 사용할지를 지정해 준다. 주로 실행시에 전략 객체를 변경할 수 있는 유연성을 제공하기 위한 용도로 쓰임.
        - `서브클래스를 만드는 방법을 대신`해 유연성을 극대화하기 위한 용도로 사용됨
        - 구성을 통해 행동을 정의하는 객체를 유연하게 바꿀 수 있음
        - 어떤 클래스의 인스턴스를 만들고 그 인스턴스에게 어떤 행동을 구현하는 전략 객체를 건네 줌
    - 클래스 다이어그램은 동일하나, 용도 차이

## 8. 동시성
### 프로세스 작동 방식 & 쓰레드 작동 방식
[[Java] Thread](/study/2021/02/22/Java-Thread/)



## 9. OS
### Linux 메모리 꽉 찰 경우 발생하는 현상 - swap memory
[[Linux] swap memory](/study/2021/06/05/Linux-swap-memory/)

## 10. ETC
### 웹 처리 방식, 순서
### NGINX 킵얼라이브
### WAS-WS
### CSRF
### BulkHead pattern
- [https://techblog.woowahan.com/2542/](https://techblog.woowahan.com/2542/)
- [https://github.com/Netflix/Hystrix/wiki/How-it-Works#threads--thread-pools](https://github.com/Netflix/Hystrix/wiki/How-it-Works#threads--thread-pools)