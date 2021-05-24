---
layout: post
title: "[Interview] 면접 공부"
subtitle: "interview"
categories: study
tags: etc
---
> 면접 공부  

# 1. Java
## 1.1. HashMap & TreeMap
[[Java] HashMap vs TreeMap](/study/2021/05/24/Java-HashMap-vs-TreeMap)

## 1.2. primitive & reference
## 1.3. Boxing & Unboxing
[[Java] Primitive & Reference & Boxing & Unboxing](/study/2021/05/24/Java-Primitive-&-Reference-&-Boxing-&-Unboxing)


## 1.4. JVM 메모리
[[Java] JVM Stack & Heap](/study/2021/05/24/Java-JVM-Stack-&-Heap)

## 1.5. Generic
> 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입 체크를 해주는 기능  

### 🚀Generics의 장점
 - 타입 안정성 제공
 - 타입 체크와 형변환의 생략 -> 코드 간결해짐.

### 🚀타입 안정성?
- 의도하지 않은 타입의 객체 저장 막음
- 저장된 객체 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄일 수 있음


 즉, Generics는 **다룰 객체의 타입을 미리 명시해줌으로써 번거로운 형변환을 줄여주는 것**이다.


# 2. HTTP
## 2.1. POST & PUT 동일 요청 반복 경우 어떠한 결과가 나오는가

### 🚀idempotent?
> f(x) = f(f(x))  

우리나라 말로 멱등법칙 또는 멱등성이라 한다. 연상을 여러 번 적용하더라도 결과가 달라지지 않는 성질을 의미한다.  
POST와 PUT의 경우 멱등성에 있어 차이를 보인다. POST는 idempotent하지 않다.  

### 🚀POST
리소스 생성에 사용되는 HTTP 메서드.  
반복해서 요청할 경우 서로 다른 id를 가진 리소스를 생성한다. 즉, **idempotent 하지 않다**. 

### 🚀PUT
특정 리소스에 대한 수정을 요청할 때 사용되는 HTTP 메서드.  
반복해서 요청을 하더라도 동일한 결과를 보장한다. 즉, **idempotent** 하다.  

### 🚀2.1. 참조
- [http://1ambda.github.io/javascripts/rest-api-put-vs-post/](http://1ambda.github.io/javascripts/rest-api-put-vs-post/)


## 2.2. Redirection
### 🚀3xx (Redirection)
> 요청을 완료하기 위해 유저 에이전트(웹 브라우저)의 추가조치 필요  

웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동  

### 🚀Redirection 종류 3가지
#### 💻1. 영구 리다이렉션
- 특정 리소스의 URI가 영구적으로 이동
- 원래의 URL을 사용❌, 검색 엔진 등에서도 변경 인지
- HTTP 상태 코드
    1. `301` Moved Permanently
        - 리다이렉트시 요청 메서드가 **GET**으로 변하고, 본문이 제거될 수 있음
    2. `308` Permanent Redirect
        - 301과 기능 동일
        - 리다이렉트시 요청 메서드와 본문 유지
- ie) /members → /users
- ie) /event → /new-event

#### 💻2. 일시 리다이렉션
일시적인 변경  
- 리소스의 URI가 일시적으로 변경
- 검색 엔진 등에서 URL을 변경하면 안됨
- HTTP 상태 코드
    1. `302` Found
        - 리다이렉트시 요청 메서드가 **GET**으로 변하고, 본문이 제거될 수 있음
    2. `307` Temporary Redirect
        - 302와 기능 동일
        - 리다이렉트시 요청 메서드와 본문 유지(요청 메서드를 변경하면 안된다)
    3. `303` See Other
        - 302와 기능 동일
        - 리다이렉트시 요청 메서드가 GET으로 변경
- PRG : Post/Redirect/Get
    - POST로 주문 후에 새로 고침으로 인한 중복 주문 방지
    - POST로 주문 후에 주문 결과 화면을 GET 메서드로 리다이렉트
    - 새로고침해도 결과 화면을 GET으로 조회
    - 중복 주문 대신 결과 화면만 GET으로 다시 요청
- 302 ? 307 ? 303 ? 어떤 것 사용?
    - 302 : GET으로 변경 될 수 있음
    - 307 : 메서드가 변하면 안 됨
    - 303 : 메서드가 GET으로 변경
    - 원칙
        - 302 스펙의 의도는 HTTP 메서드 유지, 허나 웹 브라우저 대부분 302 코드에 대해 GET으로 변경
        - 모호한 302 대신 307, 303 등장
    - 현실
        - 구체적인 307, 303 사용을 권장하나 많은 애플리케이션 라이브러리들이 302 기본 사용
        - 자동 리다이렉션시 GET으로 변해도 되면 302 사용해도 큰 문제 ❌

#### 💻3. 특수 리다이렉션
결과 대신 `캐시`를 사용  
- HTTP 상태 코드
    1. `300` Multiple Choices
        - 사용 ❌
    2. `304` Not Modified
        - 캐시를 목적으로 사용
        - 클라이언트에게 리소스가 수정되지 않았음을 알려줌. 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용(캐시로 리다이렉트)
        - 304 응답은 응답에 메시지 바디를 포함하면 ❌ (로컬 캐시 사용해야 하기 때문)
        - 조건부 GET, HEAD 요청시 사용

### 🚀2.2.참조
- [https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections](https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections) 
- 인프런, 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC#)


# 3. 네트워크
## 3.1. Load-Balancer
[[Network] Load Balacing](/study/2021/05/24/Network-Load-Balacing)

## 3.2. TCP vs UDP
[[Network] TCP & UDP](/study/2021/05/24/Network-TCP-&-UDP)

# 4. 데이터베이스
## 4.1. 복합키

## 4.2. Sharding & Replication
### 🚀Partitioning
**파티셔닝**은 일반적으로 DB 테이블을 작은 부분으로 여러 개 나누는 것을 가리킴. Column을 기준으로 나누는 `Vertical Partitioning`(수직)과 Row를 기준으로 나누는 `Horizontal Partioning`(수평)이 있다.

### 🚀Sharding
**샤딩**은 파티셔닝의 수평 파티셔닝의 특수한 형태. `Shard key`라고 알려진 키를 기준으로 데이터가 나뉨. Shard key 알고리즘에 따라 여러 Sharding이 있음.  
샤딩은 여러 서버에 스키마가 복제됨. 데이터는 Shard key를 기준으로 여러 노드들에 나누어 저장됨.  
검색 요청에 대한 부하를 서로 다른 샤드가 있는 여러 서버에 분산할 수 있음.  
스키마를 유지하기 때문에 DB 확장에 있어서도 적용이 쉬움.

### 🚀Replication


### 🚀4.2. 참조
- [https://www.youtube.com/watch?v=AxpsOLLQt3o](https://www.youtube.com/watch?v=AxpsOLLQt3o)

## 4.3. PreparedStatement & Statement
[[Java] Statement & PreparedStatement](/study/2021/05/24/Java-Statement-&-PreparedStatement)

# 5. 스프링
## 5.1. 커넥션 풀
## 5.2. @Transactional
## 5.3. 빈 스코프

# 6. 디자인 패턴
## 6.1. 전략패턴
## 6.2. 옵저버 패턴

# 7. 동시성
## 7.1. 프로세스 작동 방식 & 쓰레드 작동 방식

# 8. OS
## 8.1. Linux 메모리 꽉 찰 경우 발생하는 현상