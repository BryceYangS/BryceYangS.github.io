---
layout: post
title: "[Design Pattern]옵저버 패턴"
subtitle: "디자인 패턴 옵저버 패턴"
categories: study
tags: oop
---
> 디자인 패턴의 `옵저버 패턴` 정리

# Observer

`옵저버 패턴`에서는 한 객체의, 상태가 바뀌면 그 객체에 의존하느 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로 `일대다`(one-to-many) 의존성을
정의합니다.

일대다 관계는 주제와 옵저버에 의해 정의된다. 옵저버 패턴을 구현하는 방법에는 여러 가지가 있지만, 대부분 주제(Subject) 인터페이스와 옵저버(Observer) 인터페이스가
들어있는 클래스 디자인을 바탕으로 함.

![observer](/assets/img/oop/observer.jpg)

## Java 내장 옵저버 패턴 사용

- 자바의 내장 클래스 `java.util.Observer` & `java.util.Observable` 을 사용해서 옵저버 패턴을 구현할 수도 있음
- 주제 객체(Subject, Publisher)는 Observable 클래스를 extends
- 옵저버(Observer, Subscriber)는 Observer 인터페이스 implements

**Observer, Observable은 9버전부터는 deprecated**

Observable의 단점

1. 클래스
    - 서브 클래스를 생성해서 사용해야 함. 클래스이기 때문에 다중 상속 불가. 재사용성에 제약이 발생함ㅁ
    - Observable 인터페이스라는 것이 없기 때문에 자바에 내장된 Observer API하고 잘 맞는 클래스를 직접 구현하는 것이 불가능

2. Observable 클래스의 핵심 메소드를 외부에서 호출 불가
    - setChanged() 메소드의 접근 제어가 protected : 서브클래스에서만 호출 가능

이러한 단점으로 옵저버 패턴을 직접 구현해서 사용하는 경우도 있음. 또한 이 후 Java는 `java.beans` 패키지를 통해 이벤트 모델을 제공하고 있음.

그 외에도 멀티 스레드에서 순서가 보장되며 신뢰할 수 있는 메시징을 `java.util.concurrent` 패키지의 자료 구조들 중 하나를 사용하는 것을 고려할 수 있음.

reactive stream 스타일의 프로그래밍은 `Flow` API 사용.

## 1. 디자인 원칙

1. 서로 상호작용을 하는 객체 사이에서는 가능하면 `느슨하게 결합`하는 디자인을 사용
    - 두 객체가 느슨하게 결합 : 그 둘이 상호작용을 하긴 하지만 서로에 대해 서로 잘 모른다는 것을 의미

    1. 주제가 옵저버에 대해서 아는 것은 옵저버가 특정 인터페이스(Observer 인터페이스)를 구현한다는 것 뿐.
    2. 옵저버는 언제든지 새로 추가 가능
    2. 새로운 형식의 옵저버를 추가하려고 할 때도 주제를 전혀 변경할 필요 없음
    2. 주제와 옵저버는 서로 독립적으로 재사용 가능
    2. 주제나 옵저버가 바뀌더라도 서로한테 영향을 미치지 않음
   

