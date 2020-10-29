---
layout: post
title: "[Java]Generics"
subtitle: "Generics"
categories: study
tags: java
---

> 남궁 성님의 `Java의 정석` 책의 Generics 부분을 정리한 포스팅입니다.

## Generics

### Generics란?
> 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입 체크를 해주는 기능

Generics의 장점
 - 타입 안정성 제공
 - 타입 체크와 형변환의 생략 -> 코드 간결해짐.

 ```
 타입 안정성?
  - 의도하지 않은 타입의 객체 저장 막음
  - 저장된 객체 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄일 수 있음
 ```

 즉, Generics는 **다룰 객체의 타입을 미리 명시해줌으로써 번거로운 형변환을 줄여주는 것**이다.

 ### Generic Class 선언
**Generics 적용 전** 
```java
class Box {
    Object item;

    void setItem(Object item) { this.item = item; }
    Object getItem() { return this.item; } 
```

**Generics 적용 후**
```java
class Box<T> {
    T item;
   
    void setItem(T item) { this.item = item; }
    T getItem() {return this.item; }
}
```

**Generics 적용 후 객체 생성**
```java
Box<String> b = new Box<String>(); // T 자리에 실제 타입 지정
b.setItem(new Object()); // 에러 발생. String 제외 타입 지정 불가.
b.setItem("abc");
String item = (String) b.getItem();
```

***Generics 용어***
```
Box<T> : Generic class. 'T의 Box' 또는 'T Box'라고 읽는다.
T      : 타입 변수 또는 타입 매개 변수
Box    : 원시 타입
```

- Generics 제한
    + static멤버에 타입 변수 T 사용 불가 : T는 인스턴스변수로 간주됨
    ```java
    class Box<T> {
        static T item; //에러
        ststic int compare(T t1, T t2) { ... } // 에러
    }
    ```
    + Generic 타입의 배열 생성 불가