---
layout: post
title: "[Test] ArchUnit 의존성 테스트"
subtitle: "archunit"
categories: study
tags: test
---
> ArchUnit

## ArchUnit?
시스템의 아키텍처를 정의하고, 이에 맞춰 코드를 작성해야 함. 아키텍처 준수 여부에 대한 확인할 수 있는 기능을 제공하는 것이 `ArchUnit`.  

즉, ArchUnit은 **애플리케이션이 주어진 아키텍처 규칙을 준수하는지 확인할 수 있는 테스트 라이브러리**.



## ArchUnit 기능

1. 패키지 간 의존 관계
2. 클래스 간 의존 관계
3. 클래스와 패키지 포함 관계
4. 상속 관계 검사
5. 어노테이션 검사
6. 레이어 아키텍처 검사
7. 순환 참조 검사



## ArchUnit Dependency

JUnit4 : <u>archunit-junit4</u> 의존성 추가

```xml
<dependency>
    <groupId>com.tngtech.archunit</groupId>
    <artifactId>archunit-junit4</artifactId>
    <version>0.23.1</version>
    <scope>test</scope>
</dependency>
```

```gradle
testImplementation 'com.tngtech.archunit:archunit-junit4:0.23.1'
```

JUnit5 : arch unit-junit5 의존성 추가

```xml
<dependency>
    <groupId>com.tngtech.archunit</groupId>
    <artifactId>archunit-junit5</artifactId>
    <version>0.23.1</version>
    <scope>test</scope>
</dependency>
```

```gradle
testImplementation 'com.tngtech.archunit:archunit-junit5:0.23.1'
```



## ArchUnit 예시

![archunit-example](/assets/img/java/archunit/archunit_example.png)

### user 패키지

```java
@Test
void user_package() {
  JavaClasses classes = new ClassFileImporter().importPackages("com.example.demo");
  ArchRule orderPackageRule = classes().that().resideInAPackage("..user..")
    .should().onlyBeAccessed().byClassesThat().resideInAPackage("..user..");
  orderPackageRule.check(classes);
}
```








## 참조
- https://www.archunit.org/userguide/html/000_Index.html
- https://www.baeldung.com/java-archunit-intro
- https://d2.naver.com/helloworld/9222129