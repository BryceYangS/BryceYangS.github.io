---
layout: post
title: "[Spring] SpringBoot 3 버전업으로 인한 hibernate 6의 @GeneratedValue 전략 변경"
subtitle: "spring 6, spring boot 3, hibernate 6"
categories: study
tags: springboot
---

> SpringBoot 3.x 버전업으로 인한 hibernate 6.x @GeneratedValue 전략 변경

## 문제 상황
spring-data-jpa 3.x 버전 업그레이드로 hibernate 6.x 버전업  

```java
@Entity
public class Person {
    @Id
    @GeneratedValue
    Long id;    
}
```

기존 `@GeneratedValue` 사용 시 hibernate는 `hibernate_sequence` 테이블 통해 id 값 부여 전략.  
springboot 3.x 버전 업그레이드 시 `테이블명_seq` 를 이용하게 되면서 shema validation에서 실패 발생.

### 시퀀스 네이밍 전략 변경 (since. hibernate 6)
hibernate 6부터 `hibernate.id.db_structure_naming_strategy` 프로퍼티 설정 가능. (기본값 ***standard***)  
네이밍 전략은 4가지 존재  

- standard
    - hibernate 6 부터 도입. 기본 값
    - 엔티티 클래스와 매핑되는 테이블에 suffix `_seq`
- legacy
    - hibernate 5.3 이상 6.x 미만 기본값.
    - pk 생성기 명시할 수 있음. 명시하지 않은 경우, `hibernate_sequence` 사용
    ```java
    @Entity
    public class Person {
        @Id
        @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "people_seq")
        @SequenceGenerator(name="people_seq", sequenceName = "people_seq")
        private Long id;
    }
    ```
- single
    - hibernate 5.3 미만 기본.
    - `hibernate_sequence` 사용
- *ImplicitDatabaseObjectNamingStrategy* 구현 클래스


#### 1. standard
hibernate 6 부터는 각 엔티티 별로 시퀀스를 분리.

```java
@Entity
public class Reservation {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private Long id;
    private LocalDateTime reservedTime;
}
```

- 기본 설정인 경우 `reservation_seq` 로 시퀀스 사용.
- 만약 `@Table(name="reserve")` 와 같이 테이블 이름 명시했을 경우, `reserve_seq`를 사용

#### 2. legacy
5.3 \<= hibernate version \< 6.x  
`hibernate.id.db_structure_naming_strategy` 에 **legacy** 값 설정.  
`@GeneratedValue` or `@GeneratedValue(strategy = GenerationType.SEQUENCE)` 사용할 경우  ***hibernate_sequence*** 를 사용  
5.3 부터는 generator를 명시할 수 있음. `@GeneratedValue(strategy = GenerationType.SEQUENCE, generator="player_seq")` 경우 ***player_seq*** 사용  


#### 3. single
***hibernate_sequence*** 무조건 사용

#### 4. ImplicitDatabaseObjectNamingStrategy 구현
- 검색해보니... 거의 쓰지 않는듯?



## 추가적으로.. UUID 새로운 접근법 도입
과거에는 UUID 부여 전략이 hibernate 내부 함축적이었음. 6 버전부터 명시적으로 설정할 수 있게 바뀌었음
```java
@Id
@UuidGenerator(style = UuidGenerator.Style.TIME)
@GeneratedValue
private UUID id;
```

### Auto/Random 
- 기본 전략.
- UUID.randomUUID() 메서드 사용

### Time
- IETF RFC 4122 기반의 시간 기반 생성 전략 . 
- MAC 주소 보다 IP 주소 사용.
- 이전 전략 보다 느림

# References
- [Sequence naming strategies in Hibernate 6](https://thorben-janssen.com/sequence-naming-strategies-in-hibernate-6/#Naming_strategies_supported_by_Hibernate_6)
- [Hibernate 6 - what's new and why it's important](https://www.jpa-buddy.com/blog/hibernate6-whats-new-and-why-its-important/)