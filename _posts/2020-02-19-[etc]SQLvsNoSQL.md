---
layout: post
title: "[etc]SQL vs NoSQl"
subtitle: "SQL vs NoSQL"
categories: study
tags: etc
---

# 1.1. SQL vs NoSQL

- SQL과 NoSQL은 상황에 따라 적용해야함. 무조건적으로 NoSQL이 좋다고 할 수 없음.
- NoSQL (Non Relational Operation Database SQl) : 비관계형 데이터 베이스

## 1.1.1. [SQL (관계형 데이터베이스)]

특징

1. 엄격한 schema : 스키마를 준수하지 않는 레코드 추가 불가
2. 관계 : 데이터는 여러개의 테이블에 분산되며 서로 관계를 가짐

**1. 엄격한 schema**

![SQLSchema](https://academind.com/static/c6c8b088e9d9dd4722a965cde6b76e0d/d7ad1/sql-schema.jpg)

- 데이터는 record로 테이블에 저장됨
- 테이블은 명확히 정의된 구조를 가짐
  - 구조: 필드의 집합. 필드의 이름과 데이터타입으로 정의됨
  - 필드: 어떤 데이터가 테이블에 들어갈지, 말지를 정함
- shema에 부적합한 데이터는 테이블의 record로 추가 불가

**2. 관계**
![SQL relations](https://academind.com/static/5df24f0f34a3d98feb531b5fc7776f72/a2510/sql-relations.jpg)

- 데이터 중복을 피하기 위해 여러 테이블로 데이터를 나눔.
- 이러한 구조의 장점 : 부정확한 데이터 삽입 발생 X

> SQL에선 정해진 schema를 따르지 않으면 데이터 추가 불가

## 1.1.2. NoSQL (비관계형 데이터베이스)

- NoSQL의 Collection은 SQL의 Table
- NoSQL의 document는 SQL의 record
- document는 JSON과 비슷한 형태
- 일반적으로 관련 데이터를 동일한 Collection에 넣음

![NoSQL](https://academind.com/static/986e1a479cf96ceec8a23a0386111fe9/4b190/nosql-no-schema.jpg)

특징

- schema 無
- 관계 無

> NoSQL에선 다른 구조의 데이터를 같은 컬렌션에 추가 가능

## 1.1.3. 수직적, 수평적 확장


- 수직적 확장
  - 데이터베이스 서버의 성능을 향상 시킴(CPU 업그레이드 등)
- 수평적 확장
  - 서버가 추가되고 데이터베이스가 전체적으로 분산
  - 하나의 데이터베이스에서 여러 호스트 작동

![Vertical & Horizontal Scailing](https://academind.com/static/3d5e1fd10206c1c76da6214c01c7a5f4/a2510/horizontal-and-vertical-scaling.jpg)

- 일반적으로 SQL DB는 수직적 확장만 지원. 수평적 확장은 NoSQL DB에서만 가능
  - SQL의 Sharding : 제한이 많고 구현의 어려움 존재
  - NoSQL은 기본적으로 지원. 쉽게 여러 서버에서 DB를 나눌 수 있음

## 1.1.4. 선택


- <u>어떤 데이터를 다루는지</u>, <u>어떤 애플리케이션에서 사용되는지</u>를 종합적으로 고려

SQL의 장점

<ol>
    <li>명확하게 정의된 schema로 데이터 무결성 보장</li>
    <li>관계는 각 데이터를 중복없이 한 번만 저장</li>
</ol>

SQL의 단점

<ol>
    <li>NoSQL보다 덜 유연 : 데이터 schema는 사전에 계획되어야</li>
    <li>Join문으로 인해 복잡한 쿼리</li>
    <li>수평적 확장의 어려움 : 특정 시점에서 처리할 수 있는 데이터 양과 관련해 한계가 발생할 수 있음</li>
</ol>

NoSQL의 장점

<ol>
    <li>불필요한 join 최소화</li>
    <li>유연성있는 서버 구조 제공</li>
    <li>분산처리 및 병렬처리 기능</li>
    <li>수직 및 수평 확장 가능 : 애플리케이션에서 발생시키는 모든 Read/Write 요청 처리 가능</li>
</ol>

NoSQL의 단점

<ol>
    <li>데이터가 여러 컬렉션에 중복 : 수정을 해야하는 경우 모든 컬렉션에서 수행해야 함</li>
</ol>

> SQL은 언제 사용?
>
> > 관계를 맺고있는 데이터가 자주 변경되는 애플리케이션일 경우
> > 변경될 여지가 없고, 명확한 스키마가 사용자와 데이터에게 중요한 경우

> NoSQL은 언제 사용?
>
> > 정확한 데이터 구조를 알 수 없거나 변경 또는 확장될 수 있는 경우
> > 읽기 처리를 자주하지만, 데이터를 자주 변경하지 않는 경우
> > 데이터베이스를 수평으로 확장해야하는 경우(막대한 양의 데이터 다룰 경우)

### 참조


- SQL vs NoSQL : https://academind.com/learn/web-dev/sql-vs-nosql/
